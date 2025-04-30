<template>
  <div class="h-full content-center justify-items-center">
    <el-container
      class="result-container h-5/6 w-4/5 space-x-14 bg-white pr-10"
    >
      <el-aside
        width="10%"
        class="content-center justify-items-center bg-[#0C1136] hover:cursor-pointer"
        @click="$router.push('/')"
      >
        <div
          class="ml-4 h-9 w-9 rotate-45 border-b-[1px] border-l-[1px] border-solid border-b-[white] border-l-[white]"
        ></div>
      </el-aside>
      <el-container class="py-8">
        <el-header class="my-4 space-y-4" height="auto">
          <div class="flex items-center">
            <img src="@/assets/svg/logo.svg" class="w-6" alt="icon" />
            <el-divider direction="vertical" />
            <span class="text-2xl font-bold text-[#0C1136]">{{
              headerData.title.toUpperCase()
            }}</span>
          </div>
          <div class="grid grid-cols-6 gap-x-4">
            <el-input
              v-model="searchText"
              class="search-input col-span-3"
              :placeholder="headerData.search.placeHolder"
              v-on:keydown.enter="searchWithSchool"
            >
              <template #append>
                <el-button @click="searchWithSchool">
                  <template #icon>
                    <searchIcon color="#FFFFFF" />
                  </template>
                </el-button>
              </template>
            </el-input>
            <div
              v-for="(item, i) in headerData.attributes"
              :key="i"
              class="col-span-1"
            >
              <label
                class="block text-sm font-normal text-[#0C1136]"
                :for="item.name"
                >{{ item.label }}</label
              >
              <span
                class="text-3xl font-normal text-[#0C1136]"
                :name="item.name"
                >{{ route.query[item.name] }}</span
              >
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
          <el-table
            :data="dataTable"
            :border="parentBorder"
            :preserve-expanded-content="preserveExpanded"
            :default-sort="{ prop: 'scoreWithBlock', order: 'descending' }"
            height="80%"
            style="width: 100%"
          >
            <!-- <el-table-column type="expand">
              <template #default="props">
              </template>
            </el-table-column> -->
            <el-table-column
              label="Trường"
              sortable
              prop="nameSchool"
              min-width="170"
            />
            <el-table-column
              label="Ngành"
              sortable
              prop="field"
              min-width="170"
            />
            <el-table-column label="Điểm" sortable prop="scoreWithBlock" />
            <el-table-column width="50">
              <template #default="scope">
                <el-button
                  link
                  size="small"
                  @click.prevent="deleteRow(scope.row.id)"
                >
                  <template #icon>
                    <deleteIcon />
                  </template>
                </el-button>
              </template>
            </el-table-column>
          </el-table>
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script lang="ts" setup>
import searchIcon from "~/components/icon/searchIcon.vue";
import deleteIcon from "~/components/icon/deleteIcon.vue";
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
  id: number;
  nameSchool: string;
  field: string;
  scoreWithBlock: string;
}

const searchText = ref("");
const searchResult = ref<searchResult>();
const dataFetch = ref<searchItemResult[]>([]);
const dataTable = ref<dataTableList[]>([]);
const dataTableFetch = ref<dataTableList[]>([]);

const parentBorder = ref(false);
const preserveExpanded = ref(false);

const { year, score, block } = route.query;
await fetchData({
  year: year as string,
  score: score as string,
  block: block as string,
});

async function searchWithSchool() {
  dataTable.value = dataTableFetch.value.filter((items) => {
    return items.nameSchool
      .toLowerCase()
      .includes(searchText.value.toLowerCase());
  });
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
    if (!(data as any).success) {
      ElNotification({
        title: "Error",
        message: (data as any)?.error,
        type: "error",
      });
    } else {
      searchResult.value = data as searchResult;
      dataFetch.value = searchResult.value.data as searchItemResult[];
      let idx = 0;
      dataTableFetch.value = dataFetch.value.flatMap((item) => {
        return item.aquired.map((acquired) => ({
          id: idx++,
          nameSchool: item.nameSchool,
          field: acquired.field,
          scoreWithBlock: `${acquired.score} (${acquired.block})`,
        }));
      });
      dataTable.value = dataTableFetch.value;
    }
  } catch (error) {
    console.log(error);
  }
}
const deleteRow = (index: number) => {
  dataTable.value.splice(
    dataTable.value.findIndex((item: dataTableList) => item.id === index),
    1,
  );
};
const activeIndex = ref("1");
const handleSelect = (key: string, keyPath: string[]) => {
  console.log(key, keyPath);
};
</script>

<style lang="scss" scoped>
.result-container {
  box-shadow: 8px 8px 0px 0px #0c1136;
}

:deep(.el-input-group) {
  border: #0c1136 1px solid;
  border-radius: 0;
}

:deep(.search-input) {
  .el-input__wrapper {
    border-radius: 0;
    box-shadow: none;
  }

  .el-input__inner::placeholder {
    font-weight: 400;
    font-size: 0.8rem;
  }

  .el-input-group__append {
    border-radius: 0;
    box-shadow: none;
    background-color: #e72166;
    padding: 0;
    width: 128px;
  }
}

:deep(el-menu-item) {
  color: #0c1136;
  font-size: 0.8rem;
  font-weight: 500;
}

:deep(
  .el-table--enable-row-hover .el-table__body tr:hover > td.el-table__cell
) {
  background-color: #e72166;
  color: #ffffff;
}
</style>
