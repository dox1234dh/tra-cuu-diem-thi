<template>
  <div class="h-full content-center justify-items-center">
    <el-container class="h-5/6 w-4/5 bg-white">
      <el-aside
        width="15%"
        class="content-center justify-items-center bg-[#0C1136] hover:cursor-pointer"
        @click="$router.push('/')"
      >
        <div
          class="ml-4 h-9 w-9 rotate-45 border-b-[1px] border-l-[1px] border-solid border-b-[white] border-l-[white]"
        ></div>
      </el-aside>
      <el-container>
        <el-header class="my-4 space-y-4" height="auto">
          <!-- Todo: Add Info to search from Query -->
          <div class="flex items-center">
            <img src="@/public/favicon.ico" class="w-6" alt="icon" />
            <el-divider direction="vertical" />
            <span>{{ headerData.title.toUpperCase() }}</span>
          </div>
          <div class="grid grid-cols-6 gap-x-4">
            <el-input
              v-model="searchText"
              class="col-span-3"
              :placeholder="headerData.search.placeHolder"
            >
              <template #append>
                <el-button :icon="searchIcon" @click="searchWithSchool" />
              </template>
            </el-input>
            <div
              v-for="(item, i) in headerData.attributes"
              :key="i"
              class="col-span-1"
            >
              <label class="block" :for="item.name">{{ item.label }}</label>
              <el-text :name="item.name">{{ route.query[item.name] }}</el-text>
            </div>
          </div>
        </el-header>
        <el-main>
          <el-menu
            :default-active="activeIndex"
            class="el-menu-demo"
            mode="horizontal"
            @select="handleSelect"
          >
            <el-menu-item index="1">Tất cả</el-menu-item>
            <el-menu-item index="2">Miền Bắc</el-menu-item>
            <el-menu-item index="3">Miền Nam</el-menu-item>
          </el-menu>
          <!-- Todo: Add table to show list of results from API -->
          <el-table
            :data="dataTable"
            :border="parentBorder"
            :preserve-expanded-content="preserveExpanded"
            height="85%"
            style="width: 100%"
          >
            <!-- <el-table-column type="expand">
              <template #default="props">
              </template>
            </el-table-column> -->
            <el-table-column label="Trường" sortable  prop="nameSchool" />
            <el-table-column label="Ngành" sortable  prop="field" />
            <el-table-column label="Điểm" sortable  prop="scoreWithBlock" />
          </el-table>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script lang="ts" setup>
import searchIcon from "~/components/icon/searchIcon.vue";
const route = useRoute();
const headerData = {
  attributes: [
    { name: "score", label: "Tổng điểm" },
    { name: "block", label: "Khối" },
    { name: "year", label: "Năm" },
  ],
  title: "Trúng tuyển nha",
  search: {
    placeHolder: "Tên học viện/trường đại học mà bạn muốn tra cứu",
  },
};

interface searchData {
  year: string;
  score: string;
  block: string;
}

interface acquiredItem {
  block: string;
  field: string;
  score: string;
}

interface searchItemResult {
  nameSchool: string;
  aquired: Array<acquiredItem>;
}

interface searchResult {
  success: boolean;
  data: Array<searchItemResult>;
}

interface dataTableList {
  nameSchool: string;
  field: string;
  scoreWithBlock: string;
}

const searchText = ref("");
const searchResult = ref<searchResult>();
const dataFetch = ref<searchItemResult[]>([]);
const dataTable = ref<dataTableList[]>([]);

const parentBorder = ref(false);
const preserveExpanded = ref(false);

onMounted(async () => {
  const { year, score, block } = route.query;
  await fetchData({
    year: year as string,
    score: score as string,
    block: block as string,
  });
});

async function searchWithSchool() {
  console.log(searchText.value);
}

async function fetchData(objSearch: searchData) {
  try {
    const loading = ElLoading.service({
      lock: true,
      text: "Loading",
      background: "rgba(0, 0, 0, 0.7)",
    });
    const data = await $fetch(
      "https://script.google.com/macros/s/AKfycbxXdG0mUmEfdmZOAy8TW79N5iNZKS8AWg9AWKHvh_ZVmcCINTDjToZTECecoiyIS2Cyaw/exec",
      {
        method: "GET",
        params: {
          year: objSearch.year,
          score: objSearch.score,
          block: objSearch.block,
        },
      },
    );
    loading.close();
    console.log(data);
    if (!(data as any).success) {
      ElNotification({
        title: "Error",
        message: (data as any)?.error,
        type: "error",
      });
    } else {
      searchResult.value = data as searchResult;
      dataFetch.value = searchResult.value.data as searchItemResult[];
      console.log("Fetched data:", dataFetch.value);
      dataTable.value = dataFetch.value.flatMap((item) => {
        return item.aquired.map((acquired) => ({
          nameSchool: item.nameSchool,
          field: acquired.field,
          scoreWithBlock: `${acquired.score} (${acquired.block})`,
        }));
      });
    }
  } catch (error) {
    console.log(error);
  }
}
const activeIndex = ref('1')
const handleSelect = (key: string, keyPath: string[]) => {
  console.log(key, keyPath)
}
</script>

<style></style>
