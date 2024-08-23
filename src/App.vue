<template>

  <body class="bg-[#f0f1f9]">
    <div class=" flex w-full h-full justify-between">
      <SideBar @update-tittle="getData" />
      <div v-if="tittle === 'Employee' && employeeDummy != undefined" class="grow">
        <!--employee-->

        <Employee :tittle="tittle" :employees="employeeDummy" @data-change="handleEmployeeDataChange"
          :isReset="isReset" />
      </div>
      <div v-else-if="tittle === 'Other'">
        <!--employee-->
      </div>
      <div class="flex-none flex-row w-1/6 items-center mt-4">
        <img class="m-4" src="./assets/user_logo.svg" alt=""> <!--User logo-->
        <div>
          <!--Notification-->
        </div>
      </div>
    </div>


    <div>
      <div v-if="isChange">
        <div class=" fixed bottom-0 left-0 w-full bg-white h-24 z[1000] drop-shadow-md">
          <div class=" flex flex-row justify-between w-full h-full items-center p-3">
            <p>{{ changeMsg }}</p>
            <div>
              <button @click="resetData" class="mx-4 bg-[#f0f1f9] py-2 px-6 rounded-md font-bold">Discard</button>
              <button @click="updateData" class="mx-4 bg-[#32c8c7] text-white py-2 px-6 rounded-md font-bold">Save
                changes</button>
            </div>
          </div>

        </div>
      </div>
    </div>
    <div v-if="isBusy">
      <Loading />
    </div>
    <div v-if="isShowMsgBox">
      <MessageBox :msg="msgBoxTittle" @close-msg-box="closeMessageBox" />
    </div>
    <div>

    </div>
  </body>


</template>

<script setup>
import { ref } from 'vue'
import './style.css'
import SideBar from './components/SideBar.vue'
import Employee from './components/EmployeeDetail.vue'
import axios from 'axios'
import Loading from './components/Loading.vue'
import MessageBox from './components/MessageBox.vue'

const tittle = ref("ww")
const employeeDummy = ref(undefined)
const newEmployeeData = ref({})
const isChange = ref(false)
const changeMsg = ref("")
const isReset = ref(false)
const isBusy = ref(false)
const isShowMsgBox = ref(false)
const msgBoxTittle = ref("")

const handleEmployeeDataChange = (emitData) => {
  changeMsg.value = emitData.msg
  newEmployeeData.value = emitData.data
  isChange.value = emitData.isChange
  isReset.value = false
}

const closeMessageBox = (value) => {
  isShowMsgBox.value = false;
}

const updateData = () => {
  const serverData = newEmployeeData.value.map(
    (data) => {
      return {
        "name": data.name,
        "department": data.department,
        "position": data.position,
        "join_date": dateToTimeStamp(data.employment_info.join_date),
        "initial_join_date": dateToTimeStamp(data.employment_info.initial_join_date),
        "terminate_date": dateToTimeStamp(data.employment_info.terminate_date),
        "type_of_contract": data.employment_info.type_of_contract,
        "work_experience": data.employment_info.work_experience,
        "recruitment_type": data.employment_info.recruitment_type,
        "service_lenght": 48,
        "exit_reason": data.employment_info.exit_reason,
        "exit_date": dateToTimeStamp(data.employment_info.exit_date),
        "resigned_tender_date": dateToTimeStamp(data.employment_info.resigned_tender_date),
        "employee_status": data.employment_info.employee_status,
        "nik": data.nik
      }
    }
  )
  isBusy.value = true;
  serverData.forEach((data) => {
    //since the free API only can update data by ID 1 by 1
    axios.put(`https://66c7b0e5732bf1b79fa73c23.mockapi.io/employee/all/EmployeeData/${data.nik}`, data)
      .then((response) => {
        employeeDummy.value = newEmployeeData.value
        isChange.value = false
      })
      .catch((error) => {
        isShowMsgBox.value = true;
        msgBoxTittle.value = "Failed to update data: " + error;
      })
      .finally(() => {
        isBusy.value = false;
      });
  });

}

const resetData = () => {
  isReset.value = true
}

const timeStampToDate = (timestamp) => {
  const date = new Date(timestamp * 1000);
  const year = date.getFullYear();
  const month = String(date.getMonth() + 1).padStart(2, '0');
  const day = String(date.getDate()).padStart(2, '0');
  return `${year}-${month}-${day}`;
}

const dateToTimeStamp = (date) => {
  const tmDate = new Date(date);
  return Math.floor(tmDate.getTime() / 1000);
}

const getData = (selectedNav) => {
  tittle.value = selectedNav;
  switch (selectedNav) {
    case "Employee":
      isBusy.value = true;
      //get data from server
      axios.get('https://66c7b0e5732bf1b79fa73c23.mockapi.io/employee/all/EmployeeData').then(response => {
        const serverData = response.data
        employeeDummy.value = serverData.map(
          data => {
            return {
              name: data.name,
              nik: data.nik,
              department: data.department,
              position: data.position,
              isShowemploymentInfo: false,
              employment_info: {
                join_date: timeStampToDate(data.join_date),
                initial_join_date: timeStampToDate(data.initial_join_date),
                terminate_date: timeStampToDate(data.terminate_date),
                type_of_contract: data.type_of_contract,
                work_experience: data.work_experience,
                recruitment_type: data.recruitment_type,
                service_length: data.service_length,
                employee_status: data.employee_status,
                exit_reason: data.exit_reason,
                exit_date: timeStampToDate(data.exit_date),
                resigned_tender_date: timeStampToDate(data.resigned_tender_date),
              }
            }
          }
        )
      }).catch(error => {
        isShowMsgBox.value = true;
        msgBoxTittle.value = "Failed to get data: " + error;

      }).finally(() => {
        isBusy.value = false;
      })

      break;

    default:
      break;
  }
}
</script>
