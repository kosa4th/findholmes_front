<template>
  <div class="request-container">
    <header class="request-header" @click="goBack">
      <button class="back-button">&lt;</button>
      <h2>의뢰서</h2>
      <span class="header-span">의뢰서를 확인하고, 답변서를 작성해주세요</span>
    </header>

    <div class="request-contain">
      <div class="request">
        <h3 colspan="2">의뢰서</h3>

        <table class="request-table">
          <tr>
            <td class="label" colspan="1">의뢰제목</td>
            <td class="value" colspan="3">{{ request.title }}</td>
          </tr>

          <tr>
            <td class="label" colspan="1">의뢰일자</td>
            <td class="value" colspan="3">
              {{ timeconvert(request.createAt) }}
            </td>
          </tr>

          <tr>
            <td class="label" colspan="1">의뢰분야</td>
            <td class="value" colspan="3">{{ request.speciality }}</td>
          </tr>
          <tr>
            <td class="label" colspan="1">의뢰지역</td>
            <td class="value" colspan="3">{{ request.location }}</td>
          </tr>

          <tr>
            <td class="label" colspan="1">탐정성별</td>
            <td class="value" colspan="3">
              {{ convertGender(request.detectiveGender) }}
            </td>
          </tr>

          <tr>
            <td class="label" colspan="4">의뢰에 대한 자세한 내용</td>
          </tr>
          <tr>
            <td class="value" colspan="4">
              <div class="description">
                <textarea v-model="request.description" readonly></textarea>
              </div>
            </td>
          </tr>
        </table>
      </div>
      <button @click="createEstimate(requestId)" class="response-btn">
        답변서 작성
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { mapGetters } from "vuex";

export default {
  props: ["requestId"],
  computed: {
    ...mapGetters(["getUser"]),
  },
  data() {
    return {
      request: [],
    };
  },
  created() {
    this.getRequestDetail();
  },
  methods: {
    goBack() {
      this.$router.go(-1);
    },
    async getRequestDetail() {
      console.log("실행시도", this.requestId);
      try {
        const response = await axios.get(
          "https://findmyholmes.store/request/detail",
          {
            params: { requestId: this.requestId },
          }
        );
        this.request = response.data;
        console.log(this.request);
      } catch (error) {
        this.assignedRequests = [];
      }
    },
    getUserInfo() {
      console.log(this.getUser);
    },
    moveToReply() {
      this.$router.push(`/detective/reply/${this.request.requestId}`);
    },
    createEstimate(requestId) {
      this.$router.push(`/detective/estimate/${requestId}`);
    },
    convertGender(detectiveGender) {
      if (detectiveGender === "MALE") {
        return "남자";
      } else if (detectiveGender === "FEMALE") {
        return "여자";
      } else {
        return "전체";
      }
    },
    timeconvert(time) {
      const converttime = new Date(time);
      const year = converttime.getFullYear();
      const month = String(converttime.getMonth() + 1).padStart(2, "0"); // 월은 0부터 시작하므로 +1
      const day = String(converttime.getDate()).padStart(2, "0");
      const hour = String(converttime.getHours()).padStart(2, "0");
      const minute = String(converttime.getMinutes()).padStart(2, "0");
      return `${year}-${month}-${day} ${hour}:${minute}`;
    },
  },
};
</script>

<style scoped>
.request-container {
  font-family: Arial, sans-serif;
}

.request-header {
  display: flex;
  align-items: center;
  cursor: pointer;
  background-color: #80808012;
}

.back-button {
  font-size: 21px;
  margin-left: 0px;
  padding: 8px 15px;
  background: none;
  border: none;
  cursor: pointer;
}

h2 {
  margin-left: -5px;
  font-size: 16px;
  font-weight: bold;
}

.header-span {
  color: #666;
  font-size: 12px;
  margin: 5px 0 0 5px;
}

h3 {
  text-align: center;
  font-size: 19px;
  letter-spacing: 5px;
}

.request-contain {
  display: flex;
  flex-direction: column;
  max-width: 800px;
  margin: 0 auto;
  padding: 20px 15px;
}

.request {
  border: 1px solid #8080803b;
  border-radius: 10px;
}

.request-table {
  width: 100%;
  border-collapse: collapse;
}

.request-table td:nth-child(1) {
  padding: 11px 0px;
  border-top: 1px solid #8080803b;
  text-align: center;
}

.request-table td:nth-child(2) {
  border-top: 1px solid #8080803b;
  padding-left: 10px;
}

.label {
  background-color: #f5f5f5;
  font-weight: bold;
  text-align: left;
  font-size: 14px;
}

.value {
  text-align: left;
  font-size: 14px;
  font-family: math !important;
}

.response-btn {
  width: 100%;
  background-color: #ffdf3e9c;
  border: 1px solid #d3cb3a5e;
  padding: 10px 0;
  border-radius: 9px;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  margin-top: 16px;
}

.description {
  height: 260px;
}

textarea {
  border: none;
  resize: none;
  width: 94%;
  height: 250px;
  line-height: 23px;
  font-family: math;
  padding: 4px 10px;
  font-size: 14px;
}

@media screen and (max-width: 768px) {
  .request-table td {
    font-size: 12px;
    padding: 6px;
  }

  .request-header {
    font-size: 14px;
  }

  .response-btn {
    font-size: 11px;
    padding: 5px 0;
  }
}

@media screen and (max-width: 480px) {
  h2 {
    font-size: 14px;
  }

  .back-button {
    font-size: 15px;
    margin-left: 0px;
    padding: 8px 15px;
    background: none;
    border: none;
    cursor: pointer;
  }

  .header-span {
    color: #666;
    font-size: 10px;
    margin: 5px 0 0 5px;
  }

  .request-table td {
    font-size: 10px;
    padding: 5px;
  }

  .response-btn {
    font-size: 10px;
    padding: 4px 0;
  }

  textarea {
    border: none;
    resize: none;
    width: 94%;
    height: 240px;
    line-height: 23px;
    font-family: math;
    padding: 4px 8px;
  }
}
</style>
