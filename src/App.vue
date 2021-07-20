<template>
  <div class="container p-3">
    <div class="card ml-auto mr-auto">
      <div class="card-header d-flex flex-row justify-content-between">
        <img src="./assets/logo.svg" class="form-header">
        <h1>Records Request</h1>
      </div>
      <div class="card-body">
        <nav class="navbar-light bg-light pt-3 pb-3 mb-2">
          <div class="d-flex flex-row justify-content-around">
            <i class="fas fa-user fa-2x" :class="{'active':step===1}"></i>
            <i class="fas fa-clipboard fa-2x" :class="{'active':step===2}"></i>
            <i class="fas fa-info-circle fa-2x" :class="{'active':step===3}"></i>
            <i class="fas fa-envelope fa-2x" :class="{'active':step===4}"></i>
          </div>
        </nav>
        <div v-if="step===1">
          <h3>Your Information</h3>
          <div class="row">
            <div class="col-lg-6">
              <div class="mb-3">
                <label for="firstName" class="form-label">First Name
                  <span class="text-danger fw-bold">*</span>
                </label>
                <input type="text" class="form-control" id="firstName">
              </div>
              <div class="mb-3">
                <label for="lastName" class="form-label">Last Name
                  <span class="text-danger fw-bold">*</span>
                </label>
                <input type="text" class="form-control" id="lastName">
              </div>
              <div class="mb-3">
                <label for="emailAddress" class="form-label">Email address
                  <span class="text-danger fw-bold">*</span>
                </label>
                <input type="email" class="form-control" id="emailAddress"
                       aria-describedby="emailHelp">
                <div id="emailHelp" class="form-text">
                  You can use your email to track your request
                </div>
              </div>
            </div>
            <div class="col-lg-6">
              <div class="mb-3">
                <label for="businessName" class="form-label">Business Name</label>
                <input type="text" class="form-control" id="businessName">
              </div>
              <div class="mb-3">
                <label for="phoneNumber" class="form-label">Phone Number</label>
                <input type="text" class="form-control" id="phoneNumber">
              </div>
              <div class="mb-3">
                <label for="address" class="form-label">Home/Business Address</label>
                <input type="text" class="form-control" id="address">
              </div>
            </div>
          </div>
        </div>
        <div v-if="step===2">
          <div class="row">
            <div class="col-lg-6">
              <div class="mb-3 d-flex flex-column">
                <h3>Report Purpose</h3>
                <div class="form-check">
                  <input type="radio"
                         name="purpose" class="form-check-input" value="public" v-model="purpose">
                  Public
                </div>
                <div class="form-check">
                  <input type="radio"
                         name="purpose" class="form-check-input" value="media" v-model="purpose">
                  Media
                </div>
                <div class="form-check">
                  <input type="radio"
                         name="purpose" class="form-check-input" value="lawEnforcement"
                         v-model="purpose">
                  Law Enforcement Use Only
                </div>
                <div class="form-check">
                  <input type="radio"
                         name="purpose" class="form-check-input" value="gilbert" v-model="purpose">
                  Town of Gilbert
                </div>
                <div class="form-check">
                  <input type="radio"
                         name="purpose" class="form-check-input" value="other" v-model="purpose">
                  Other
                </div>
              </div>
              <div class="mb-3" v-if="purpose==='other'">
                <label for="otherReason" class="form-label">Reason for Request</label>
                <input type="text" class="form-control" id="otherReason">
              </div>
            </div>
            <div class="col-lg-6">
              <div class="mb-3 d-flex flex-column">
                <h3>Request Type</h3>
                <div class="row mb-3">
                  <div class="col d-flex flex-column">
                    <div class="form-check">
                      <input type="checkbox"
                             name="type" class="form-check-input" v-model="type.report">
                      Report
                    </div>
                    <div class="form-check">
                      <input type="checkbox"
                             name="type" class="form-check-input" v-model="type.nameSearch">
                      Name Search
                    </div>
                    <div class="form-check">
                      <input type="checkbox"
                             name="type" class="form-check-input"
                             v-model="type.audio">
                      Dispatch Audio
                    </div>
                    <div class="form-check">
                      <input type="checkbox"
                             name="type" class="form-check-input" v-model="type['911']">
                      911
                    </div>
                    <div class="form-check">
                      <input type="checkbox"
                             name="type" class="form-check-input" v-model="type.other">
                      Other
                    </div>
                  </div>
                  <div class="col d-flex flex-column">
                    <div class="form-check">
                      <input type="checkbox"
                             name="type" class="form-check-input" v-model="type.property">
                      Property
                    </div>
                    <div class="form-check">
                      <input type="checkbox"
                             name="type" class="form-check-input" v-model="type.premiseHistory">
                      Premise History
                    </div>
                    <div class="form-check">
                      <input type="checkbox"
                             name="type" class="form-check-input"
                             v-model="type.camera">
                      Body Worn Camera (Presumptive redaction includes at minimum a medium blur)
                    </div>
                  </div>
                </div>
                <div class="mb-3" v-if="type.other">
                  <label for="otherType">Please explain</label>
                  <input type="text" v-if="type.other" class="form-control" id="otherType">
                </div>
              </div>
            </div>
          </div>
        </div>
        <div v-if="step===3">
          <div v-if="reportDetails">
            Report Details Required
          </div>
        </div>
      </div>
      <div class="card-footer">
        <div class="d-flex flex-row justify-content-between">
          <button class="btn btn-info"
                  :disabled="step===1"
                  @click.stop.prevent="step=step-1">
            <i class="fas fa-arrow-left"></i> Back
          </button>
          <button class="btn btn-info"
                  v-if="step<4"
                  @click.stop.prevent="step=step+1">
            Next <i class="fas fa-arrow-right"></i>
          </button>
          <button class="btn btn-success"
                  v-if="step===4"
                  @click.stop.prevent>
            Submit
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { computed, ref } from 'vue';

export default {
  name: 'App',
  components: {},
  setup() {
    const step = ref(1);
    const purpose = ref('public');
    const type = ref({
      report: false,
      nameSearch: false,
      audio: false,
      911: false,
      other: false,
      property: false,
      premiseHistory: false,
      camera: false,
    });
    const reportDetails = computed(() => type.value.report || type.value.audio || type.value['911'] || type.value.property || type.value.camera);
    return {
      purpose,
      reportDetails,
      step,
      type,
    };
  },
};
</script>

<style scoped>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.card-header {
  background: #242938;
  color: rgb(235, 228, 210);
}

.card-header img {
  max-height: 60px;
}

.card-header h1 {
  letter-spacing: -2px;
  text-shadow: 1px 1px 0 black;
}

i.active {
  color: #0d6efd;
}
</style>
