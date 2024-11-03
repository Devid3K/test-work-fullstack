<template>
  <n-config-provider :theme-overrides="themeOverrides">
    <n-message-provider>
      <LayoutMain class="noto-serif-thai">
        <div class="flex flex-col w-full -mt-2 mr-4">
          <div class="text-white flex whitespace-nowrap  mb-4">
            <h1 class="text-2xl ">รายการทะเบียนพิเศษ</h1>
          </div>
          
          <!-- <BaseTable :dataTable="dataTable" /> -->
          <h1 class="text-red-500 underline text-center mb-4 text-3xl">Auto Delete List</h1>
          <div class="m-auto mb-4"><AutoTransFer/></div>
          
          <h1 class="text-red-500 underline text-center mb-4 text-3xl">Mapping Data From API</h1>
          <div class="grid grid-cols-5">
          
    <div v-for="(departmentData, departmentName) in dataDepartment" :key="departmentName">
      <pre class="text-white">{{ departmentName }}: {{ departmentData }}</pre>
    </div>
  </div>
        </div>
      </LayoutMain>
      
    </n-message-provider>
  </n-config-provider>
</template>

<script>
import { defineComponent, ref,onMounted } from "vue";
import { NMessageProvider, NConfigProvider } from "naive-ui";
import axios from "axios";

import LayoutMain from './components/layout/LayoutMain.vue';
import BaseTable from "./components/table/BaseTable.vue";
import AutoTransFer from "./components/auto-transfer/AutoTransFer.vue";

export default defineComponent({
  components: {
    LayoutMain,
    NMessageProvider,
    // BaseTable,
    NConfigProvider,
    AutoTransFer
  },
  setup() {
    const themeOverrides = {
      Checkbox: {
        textColor: '#ffffff',
        colorChecked: '#ffffff',
        checkMarkColor: '#000000',
      },
    };

    const dataTable = ref({
      columns: [
        {
          title: 'ทะเบียน',
          key: 'numberId',
          width: '150px',
          align: 'center'
        },
        {
          title: 'รูป',
          key: 'images',
          width: '150px',
          align: 'center'
        },
        {
          title: 'กลุ่มทะเบียน',
          key: 'groupSpecialRegistration',
          width: '150px',
          align: 'center'
        },
        {
          title: 'วันที่เพิ่ม',
          key: 'addDate',
          width: '150px',
          align: 'center'
        },
        {
          title: 'วันทีแก้ไข',
          key: 'fixDate',
          width: '150px',
          align: 'center'
        },
        {
          title: 'เจ้าของข้อมูล',
          key: 'ownUser',
          width: '150px',
          align: 'center'
        },
        {
          title: 'หน่วยงาน',
          key: 'agency',
          width: '150px',
          align: 'center'
        },
        {
          title: 'สถานะ',
          key: 'status',
          width: '150px',
          align: 'center'
        },
        {
          title: '',
          key: 'action',
          width: '150px',
          align: 'center'
        }
      ],
      row: []
    })
    const dataDepartment = ref({

    })
    const fetchRegistrations = async () => {
      try {
        const response = await axios.get('http://localhost:4000/registrations');
        dataTable.value.row = response.data; 
        console.log(dataTable);
      } catch (error) {
        console.error('Error fetching registrations:', error);
      }
    };

        const fetchDepartmentApi = async () => {
      try {
        const response = await axios.get('https://dummyjson.com/users');
        const users = response.data.users;

        dataDepartment.value = mapData(users); 
        console.log('xxx',response.data);
        console.log(dataDepartment.value);

      } catch (error) {
        console.error('Error fetching registrations:', error);
      }
    };

    function mapData(users) {
      const departments = {};

      users.forEach((user) => {
        const dept = user.company.department;
        const fullName = user.firstName + user.lastName;
        const postalCode = user.address.postalCode;
        const hairColor = user.hair.color;

        if (!departments[dept]) {
          departments[dept] = {
            male: 0,
            female: 0,
            ageRange: { min: Infinity, max: -Infinity },
            hair: {},
            addressUser: {}
          };
        }

        const deptData = departments[dept];
        if (user.gender === "male") deptData.male += 1;
        if (user.gender === "female") deptData.female += 1;

        deptData.ageRange.min = Math.min(deptData.ageRange.min, user.age);
        deptData.ageRange.max = Math.max(deptData.ageRange.max, user.age);

        if (!deptData.hair[hairColor]) {
          deptData.hair[hairColor] = 0;
        }
        deptData.hair[hairColor] += 1;

        deptData.addressUser[fullName] = postalCode;
      });

      for (const dept in departments) {
        const ageRange = departments[dept].ageRange;
        departments[dept].ageRange = `${ageRange.min}-${ageRange.max}`;
      }

      return departments;
    }
        
    onMounted(() => {
      fetchRegistrations();
      fetchDepartmentApi()
    });
    return {
      themeOverrides,
      dataTable,
      dataDepartment
    };
  }
});
</script>
