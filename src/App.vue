<template>
  <div class="container p-3">
    <div class="card ml-auto mr-auto">
      <div class="card-header d-flex flex-row justify-content-between">
<!--        <img src="./assets/logo.svg" class="form-header">-->
        <h1>Police Records Request</h1>
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
          <div v-if="reportDetails" class="card mt-1 mb-3">
            <div class="card-header">
              <h4 class="card-title">Report Details</h4>
            </div>
            <div class="card-body">
              <div class="d-flex flex-column">
                <div class="form-check mb-3">
                  <input type="checkbox"
                         name="type" class="form-check-input" v-model="reportNumberUnknown">
                  Report Number Unknown
                </div>
                <div class="d-flex flex-column" v-if="!reportNumberUnknown">
                  <div class="mb-3">
                    <label for="reportNumber">Report Number</label>
                    <input type="text" id="reportNumber" class="form-control">
                  </div>
                  <div class="form-check mb-3">
                    <input type="checkbox"
                           name="type" class="form-check-input" v-model="additionalReports">
                    There is more than 1 report number
                  </div>
                  <div class="mb-3" v-if="additionalReports">
                    <label for="additionalReports">Additional Report Number(s)</label>
                    <input type="text" id="additionalReports" class="form-control">
                  </div>
                </div>
                <div class="mb-3">
                  <label for="time">Approximate Date/Time Reported to Police</label>
                  <input type="text" id="time" class="form-control">
                </div>
                <div class="mb-3">
                  <label for="location">Incident Location</label>
                  <input type="text" id="location" class="form-control">
                </div>
                <div class="mb-3">
                  <label for="persons">Persons Involved</label>
                  <input type="text" id="persons" class="form-control">
                </div>
              </div>
            </div>
          </div>
          <div v-if="type.nameSearch" class="card mt-1 mb-3">
            <div class="card-header">
              <h4 class="card-title">Name Search Information</h4>
            </div>
            <div class="card-body">
              <div class="d-flex flex-column">
                <div class="mb-3">
                  <label for="search">Person to Search</label>
                  <input type="text" id="search" class="form-control">
                </div>
                <div class="mb-3">
                  <label for="aka">Known/Possible Aliases (AKA)</label>
                  <input type="text" id="aka" class="form-control">
                </div>
                <div class="mb-3">
                  <label for="birth">Date of Birth</label>
                  <input type="text" id="birth" class="form-control">
                </div>
                <div class="mb-3">
                  <label for="ssn">Social Security Number</label>
                  <input type="text" id="ssn" class="form-control">
                </div>
              </div>
            </div>
          </div>
          <div v-if="type.premiseHistory" class="card mt-1 mb-3">
            <div class="card-header">
              <h4 class="card-title">Premise History Information</h4>
            </div>
            <div class="card-body">
              <div class="d-flex flex-column">
                <div class="mb-3">
                  <label for="phaddress">Address</label>
                  <input type="text" id="phaddress" class="form-control">
                </div>
                <div class="mb-3">
                  <label for="from">From Date</label>
                  <input type="text" id="from" class="form-control">
                </div>
                <div class="mb-3">
                  <label for="to">To Date</label>
                  <input type="text" id="to" class="form-control">
                </div>
              </div>
            </div>
          </div>
        </div>
        <div v-if="step===4">
          <div class="d-flex flex-column">
            <h3>Report Delivery</h3>
            <div class="form-check">
              <input type="radio"
                     name="delivery" class="form-check-input" value="email" v-model="delivery">
              Email Delivery
            </div>
            <div class="form-check">
              <input type="radio"
                     name="delivery" class="form-check-input" value="mail" v-model="delivery">
              US Mail
            </div>
            <div class="form-check">
              <input type="radio"
                     name="delivery" class="form-check-input" value="pickup"
                     v-model="delivery">
              In-Person Pickup
            </div>
            <div class="mb-3" v-if="delivery==='pickup'">
              <label for="authorized">Authorized Individuals that can pickup
                the report on your behalf</label>
              <input type="text" id="authorized" class="form-control">
            </div>
            <div class="alert alert-warning mt-3 p-2">
              <div class="form-check">
                <input type="checkbox"
                       name="final" class="form-check-input">
                I hereby certify under penalty of perjury that the requested records will not be
                used for commercial purposes
              </div>
            </div>
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
    const delivery = ref('email');
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
    const reportNumberUnknown = ref(false);
    const additionalReports = ref(false);
    const reportDetails = computed(() => type.value.report || type.value.audio || type.value['911'] || type.value.property || type.value.camera);
    return {
      delivery,
      purpose,
      reportDetails,
      reportNumberUnknown,
      additionalReports,
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
