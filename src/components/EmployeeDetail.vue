<template>
    <div class="">
        <div class="flex flex-row justify-between items-center m-3 ">
            <h2 class=" text-3xl mt-2 mb-7 font-bold">{{ tittle + " details" }}</h2>
            <div class="flex flex-row">
                <div class="flex flex-row items-center py-2 mx-4 px-4 rounded-full bg-[#dfe4ef]">
                    <p>Filter</p>
                    <img src="..//assets/arrow_down.svg" alt="">
                </div>
                <img src="..//assets/search.svg" alt="">
            </div>
        </div>

        <div v-for="employee in localEmployee" :key="employee.nik" class="my-8">
            <div class="flex flex-row bg-white p-8 m-3 justify-between rounded-md items-center"> <!--Top Info-->
                <img src="../assets/samanta_picture.svg" alt="">
                <div class=" px-2">
                    <p class=" text-xl font-bold">{{ employee.name }}</p>
                    <p class=" text-xl font-bold">{{ employee.nik }}</p>
                </div>
                <div class="">
                    <p>Department: <span class=" font-bold">{{ employee.department }}</span></p>
                    <p>Position: <span class=" font-bold">{{ employee.position }}</span></p>
                </div>
            </div>

            <div class="flex flex-row bg-white p-3 m-3 justify-between rounded-md"> <!--Personal Info-->
                <h2 class=" text-3xl font-bold">Personal Info</h2>
                <button>
                    <img src="../assets/arrow_down.svg" alt="">
                </button>
            </div>

            <div class="flex flex-col bg-white p-3 m-3 rounded-md"><!--Employment Info-->
                <div class="flex flex-row w-full justify-between mb-5">
                    <h2 class=" text-3xl font-bold">Employment Info</h2>
                    <button @click="showEmploymentInfo(employee.nik, employee.isShowemploymentInfo)">
                        <img v-if="employee.isShowemploymentInfo" src="../assets/arrow_up.svg" alt="">
                        <img v-else src="../assets/arrow_down.svg" alt="">

                    </button>
                </div>
                <ol v-if="employee.isShowemploymentInfo" class="flex flex-wrap w-full justify-between mb-4">
                    <li v-for="(value, key) in employee.employment_info" :key="key"
                        class=" text-l py-2 flex[50%] flex flex-row px-3 border-b-2 border-b-gray-200 mx-1">
                        <p class="w-56 font-bold">{{ translateTitle(key) }}</p>

                        <div v-if="key === 'work_experience'">
                            <select name="work_experience" id="work_experience" class=" w-40"
                                @change="updateData(employee, key, $event.target.value)">
                                <option v-for="work_experience in workExperiences" v-bind="work_experience.id"
                                    :selected="work_experience.name == value">{{ work_experience.name }}
                                </option>
                            </select>
                        </div>

                        <div v-else-if="key === 'service_length'">
                            <input type="text" :value="getServiceLenght(employee)" disabled>
                        </div>

                        <div v-else-if="key === 'exit_reason'">
                            <select name="exit_reason" id="exit_reason" class=" w-40"
                                @change="updateData(employee, key, $event.target.value)">
                                <option v-for="exit_reason in exitReasons" v-bind="exit_reason.id"
                                    :selected="exit_reason.name == value">{{ exit_reason.name }}
                                </option>

                            </select>
                        </div>

                        <div v-else-if="key === 'type_of_contract'">
                            <input type="text" disabled :value="value" class=" w-40">
                        </div>

                        <div v-else-if="key === 'recruitment_type'">
                            <select name="recruitment_type" id="recruitment_type" class=" w-40"
                                @change="updateData(employee, key, $event.target.value)">
                                <option v-for="type in recruitmentTypes" v-bind="type.id"
                                    :selected="type.name == value">{{ type.name }}
                                </option>
                            </select>
                        </div>
                        <div v-else-if="key === 'employee_status'">
                            <select name="employee_status" id="recruitment_type" class=" w-40"
                                @change="updateData(employee, key, $event.target.value)">
                                <option v-for="status in employeeStatuses" v-bind="status.id"
                                    :selected="status.name == value">{{ status.name }}
                                </option>
                            </select>
                        </div>
                        <div v-else>
                            <input type="date" :ref="key" :id="key" :value="value" class=" w-40"
                                @change="updateData(employee, key, $event.target.value)">
                        </div>
                    </li>
                </ol>
            </div>
        </div>
    </div>
</template>

<script>
import _ from 'lodash'
import employee_image from '../assets/samanta_picture.svg'

export default {
    emits: ['data-change'],
    data() {
        return {
            isDataChange: false,
            localEmployee: [...this.employees],
            recruitmentTypes: [{ id: 0, name: "Type 1" }, { id: 1, name: "Type 2" }, { id: 2, name: "Type 3" }, { id: 3, name: "Type 4" }],
            employeeStatuses: [{ id: 0, name: "Status 1" }, { id: 1, name: "Status 2" }, { id: 2, name: "Status 3" }, { id: 3, name: "Status 4" }],
            exitReasons: [{ id: 0, name: "Reason 1" }, { id: 1, name: "Reason 2" }, { id: 2, name: "Reason 3" }, { id: 3, name: "Reason 4" }],
            workExperiences: [{ id: 0, name: "1 Year" }, { id: 1, name: "2 Year" }, { id: 2, name: "3 Year" }, { id: 3, name: "4 Year" }, { id: 4, name: "5 Year" }, { id: 5, name: "6 Year" }],
        }
    },
    props: {
        tittle: String,
        employees: Object,
        isReset: Boolean,
    },
    watch: {
        isReset(val) {
            //Function reset back data to previous value
            if (val) {
                this.localEmployee = this.employees.map(
                    (employee, index) => {
                        return {
                            ...employee,
                            isShowemploymentInfo: this.localEmployee[index].isShowemploymentInfo ||employee.isShowemploymentInfo
                        }
                    }
                )
                const oldData = this.employees.map(({ isShowemploymentInfo, ...rest }) => rest)
                const newData = this.localEmployee.map(({ isShowemploymentInfo, ...rest }) => rest)

                this.isDataChange = !_.isEqual(oldData, newData)
                const emitData = { isChange: this.isDataChange, data: this.localEmployee, msg: "Employee data has change", id: "employee" }
                this.$emit("data-change", emitData)

            }
        }
    },
    methods: {
        translateTitle: (raw) => {
            switch (raw) {
                case 'join_date':
                    return "Join Date"
                case 'terminate_date':
                    return "Terminate Date"
                case 'work_experience':
                    return "Work Experience"
                case 'service_length':
                    return "Service Lenght"
                case 'exit_reason':
                    return "Exit Reason"
                case 'resigned_tender_date':
                    return "Resigned Tender Date"
                case 'initial_join_date':
                    return "Initial Join Date"
                case 'type_of_contract':
                    return "Type of Contract"
                case 'employee_status':
                    return "Employee Status"
                case 'exit_date':
                    return "Exit Date"
                case 'recruitment_type':
                    return "Recruitment Type"

                default:

                    break;
            }
        },
        showEmploymentInfo(nik, show) {
            this.localEmployee = this.localEmployee.map((item) => {
                if (item.nik === nik) {
                    return {
                        ...item,
                        isShowemploymentInfo: !show
                    };
                }
                return item;
            });
        },
        getServiceLenght(employee) {
            //console.log(employee.employment_info.join_date);
            const joinDate = new Date(employee.employment_info.join_date)
            //console.log(joinDate.getFullYear())
            if (employee.employment_info.exit_date === "") {
                const today = new Date();
                return today.getFullYear() - joinDate.getFullYear() + " year";
            }
            const exitDate = new Date(employee.employment_info.exit_date)
            return exitDate.getFullYear() - joinDate.getFullYear() + " year";
        },
        updateData(employee, key, newValue) {
            this.localEmployee = this.localEmployee.map((item) => {
                if (employee.nik === item.nik) {
                    return {
                        ...item,
                        employment_info: {
                            ...item.employment_info,
                            [key]: newValue
                        }
                    }
                }
                return item
            })
            const oldData = this.employees.map(({ isShowemploymentInfo, ...rest }) => rest)
            const newData = this.localEmployee.map(({ isShowemploymentInfo, ...rest }) => rest)

            this.isDataChange = !_.isEqual(oldData, newData)
            const emitData = { isChange: this.isDataChange, data: this.localEmployee, msg: "Employee data has change", id: "employee" }
            this.$emit("data-change", emitData)

        },

    },
}


</script>

<style></style>