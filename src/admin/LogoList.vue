<template>
    <div class="admin-panel bg-gray-100 min-h-screen flex gap-x-28">
        <Sidebar />
        <div class="py-4 w-full px-6">
        <h1 class="text-2xl font-bold mb-6">Logolar</h1>
        <table class="w-full bg-white shadow-md rounded-lg overflow-hidden">
            <thead class="bg-gray-200 border-b border-gray-300">
            <tr>
                <th class="p-4 text-left text-gray-700">Logo Adı</th>
                <th class="p-4 text-left text-gray-700">Logo</th>
                <th class="p-4 text-left text-gray-700">İşlemler</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="logo in logos" :key="logo.id" class="hover:bg-gray-50">
                <td class="p-4 border-b border-gray-200">{{ logo.name }}</td>
                <td class="p-4 border-b border-gray-200">
                <img :src="logo.imageUrl" alt="Logo" class="h-16 w-16 object-contain" />
                </td>
                <td class="p-4 border-b border-gray-200">
                <button @click="editLogo(logo)" class="bg-yellow-500 text-white py-1 px-3 rounded hover:bg-yellow-600 focus:outline-none focus:ring-2 focus:ring-yellow-500 focus:ring-offset-2">Düzenle</button>
                <button @click="deleteLogo(logo.id)" class="bg-red-500 text-white py-1 px-3 rounded ml-2 hover:bg-red-600 focus:outline-none focus:ring-2 focus:ring-red-500 focus:ring-offset-2">Sil</button>
                </td>
            </tr>
            </tbody>
        </table>
        </div>
    </div>
</template>

<style scoped>
    /* Add additional styles if needed */
    table {
    border-collapse: collapse;
    }
    thead th {
    font-weight: 600;
    }
    tbody tr {
    transition: background-color 0.3s ease;
    }
    tbody tr:hover {
    background-color: #f9f9f9;
    }
</style>
  
  
  <script>
  import axios from 'axios';
  import Sidebar from './Sidebar.vue';
  
  export default {
    components : {
        Sidebar
    },
    data() {
      return {
        logos: []
      };
    },
    async created() {
      try {
        const response = await axios.get('https://66e7327417055714e58bb31d.mockapi.io/logos');
        this.logos = response.data;
      } catch (error) {
        console.error('Logo listeleme hatası:', error);
      }
    },
    methods: {
      async deleteLogo(id) {
        try {
          await axios.delete(`https://66e7327417055714e58bb31d.mockapi.io/logos/${id}`);
          this.logos = this.logos.filter(logo => logo.id !== id);
        } catch (error) {
          console.error('Logo silme hatası:', error);
          alert('Bir hata oluştu!');
        }
      },
      editLogo(logo) {
        // Logo düzenleme işlemleri burada yapılacak
        this.$router.push(`/admin/logos/edit/${logo.id}`);
      }
    }
  };
  </script>
  