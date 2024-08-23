<template>
    <div class=" flex-none w-1/6">
        <img src="..//assets/macra_logo.svg"  alt="" class="m-4">
        <ul class=" mt-16">
            <li v-for=" (item, index) in sideBarList" :key="index" class="mx-4 my-4 p-3 flex flex-row rounded-md"
                :class="{ 'bg-[#32c8c7] text-white': item.isSelected, 'bg-transparent text-black': !item.isSelected }">

                <img :src="item.logo" alt="" class=" me-2">
                <button @click="setSelected(index)">{{ item.name }}</button>
            </li>
        </ul>
    </div>

</template>

<script>
import image1 from '../assets/dashboard_logo.svg';
import image2 from '../assets/people_logo.svg'
import image3 from '../assets/organization_logo.svg'
import image4 from '../assets/payroll_logo.svg'
import image5 from '../assets/attendance_logo.svg'
import image6 from '../assets/leave_logo.svg'
import image7 from '../assets/approval_logo.svg'
import image8 from '../assets/portal_logo.svg'
import image9 from '../assets/company_detail.svg'
import image10 from '../assets/activity_log.svg'
import image11 from '../assets/access_level.svg'
import image12 from '../assets/macra_logo.svg'

export default {
    emits: ['update-tittle'],
    data() {
        return {
            sideBarList: [
                {
                    name: "Dashboard",
                    isSelected: false,
                    logo: image1,
                },
                {
                    name: "Employee",
                    isSelected: false,
                    logo: image2,
                },
                {
                    name: "Organization",
                    isSelected: false,
                    logo: image3,
                },
                {
                    name: "Payroll",
                    isSelected: false,
                    logo: image4,
                },
                {
                    name: "Attendance",
                    isSelected: false,
                    logo: image5,
                },
                {
                    name: "Leave",
                    isSelected: false,
                    logo: image6,
                },
                {
                    name: "Approval",
                    isSelected: false,
                    logo: image7,
                },
                {
                    name: "Portal",
                    isSelected: false,
                    logo: image8,
                },
                {
                    name: "Company Details",
                    isSelected: false,
                    logo: image9,
                },
                {
                    name: "Activity Log",
                    isSelected: false,
                    logo: image10,
                },
                {
                    name: "Access Level",
                    isSelected: false,
                    logo: image11,
                },

            ],
        }
    },
    methods: {
        setSelected(selectedIndex) {
            this.sideBarList = this.sideBarList.map((item, index) => {
                return {
                    ...item,
                    isSelected: index === selectedIndex
                }
            })
            this.$emit('update-tittle', this.sideBarList[selectedIndex].name)
        }
    }
}

</script>