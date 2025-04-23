<template>
  <div class="flex h-full w-full flex-col items-center justify-center">
    <el-row class="w-full" :gutter="20" justify="center" align="middle">
      <el-col :sm="12" :xs="24">
        <!-- Container -->
        <el-card>
          <template #header>
            <!-- header -->
            <div class="block">
              <div class="flex items-center">
                <img src="@/public/favicon.ico" class="w-6" alt="icon" />
                <el-divider direction="vertical" />
                <span>{{ headerData.title }}</span>
              </div>
              <p>{{ headerData.description }}</p>
            </div>
          </template>
          <div>
            <el-form
              ref="ruleFormRef"
              :model="formSearch"
              :rules="rules"
              label-width="auto"
              label-position="top"
              style="max-width: 600px"
            >
              <el-row
                class="my-2 w-full"
                :gutter="20"
                justify="center"
                align="middle"
              >
                <el-col :span="8">
                  <el-form-item :label="formLabel.score" prop="score">
                    <el-input
                      v-model.number="formSearch.score"
                      style="width: 100%"
                      placeholder="0.00"
                      type="number"
                      :step="0.25"
                      :min="0"
                      :max="30"
                      :suffix-icon="currencyVN"
                    />
                  </el-form-item>
                </el-col>
                <el-col :span="16">
                  <el-form-item :label="formLabel.block" prop="block">
                    <el-input
                      v-model="formSearch.block"
                      style="width: 100%"
                      placeholder="A00"
                      :suffix-icon="searchIcon"
                    />
                  </el-form-item>
                </el-col>
              </el-row>
              <el-row
                class="my-2 w-full"
                :gutter="20"
                justify="center"
                align="middle"
              >
                <el-col :span="8">
                  <el-form-item :label="formLabel.year" prop="year">
                    <el-select v-model="formSearch.year" placeholder="2010">
                      <el-option
                        v-for="item in yearList"
                        :key="item"
                        :label="item"
                        :value="item"
                      />
                    </el-select>
                  </el-form-item>
                </el-col>
                <el-col :span="16">
                  <div class="hint-container flex">
                    <el-divider direction="vertical" />
                    <p class="text-sm">{{ formLabel.hint }}</p>
                  </div>
                </el-col>
              </el-row>
              <el-row
                class="my-2 w-full"
                :gutter="20"
                justify="center"
                align="middle"
              >
                <el-col :span="24">
                  <el-form-item>
                    <el-button
                      class="w-full"
                      type="primary"
                      @click.prevent="submitForm(ruleFormRef)"
                      >{{ formLabel.submitBtn }}</el-button
                    >
                  </el-form-item>
                </el-col>
              </el-row>
            </el-form>
          </div>
          <template #footer>
            <!-- footer -->
            <div class="flex flex-col items-center justify-between">
              <el-divider />
              <p>{{ footerData.title }}</p>
            </div>
          </template>
        </el-card>
      </el-col>
      <el-col :sm="12" :xs="24">
        <!-- Quote -->
        <div class="quote-container">
          <el-text>
            {{ "Không học,".toUpperCase() }}<br />
            {{ "Chỉ có bốc cứt".toUpperCase() }}
          </el-text>
          <br />
          <el-text class="mx-1">{{
            "- Câu này quen mà".toUpperCase()
          }}</el-text>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script lang="ts" setup>
import type { FormInstance, FormRules } from "element-plus";
import currencyVN from "~/components/icon/currencyVN.vue";
import searchIcon from "~/components/icon/searchIcon.vue";
const headerData = ref({
  title: "Mới thi xong",
  description: "Bơi vào đây tra cứu nào!",
});

const footerData = ref({
  title: "© 2025 - Ý tưởng từ Mr. Trần Đô",
});

const yearList = ref([
  "2010",
  "2015",
  "2016",
  "2017",
  "2018",
  "2019",
  "2020",
  "2021",
  "2022",
  "2023",
  "2024",
  "2025",
]);

const formLabel = ref({
  score: "Điểm",
  block: "Khối",
  year: "Năm",
  submitBtn: "Kiểm tra",
  hint: "Bạn đỗ hay không là việc của bạn, chúng tôi chỉ check var xíu thôi.",
});

interface RuleForm {
  score: number;
  block: string;
  year: string;
}

const ruleFormRef = ref<FormInstance>();

const formSearch = reactive<RuleForm>({
  score: 0,
  block: "",
  year: "",
});

const rules = reactive<FormRules<RuleForm>>({
  score: [
    { required: true, message: "Điểm không được để trống", trigger: "blur" },
    { type: "number", message: "Điểm phải là số", trigger: "blur" },
    {
      type: "number",
      min: 0,
      max: 30,
      message: "Điểm không hợp lệ",
      trigger: "blur",
    },
  ],
  block: [
    { required: true, message: "Khối không được để trống", trigger: "blur" },
    { min: 2, max: 3, message: "Khối không hợp lệ", trigger: "blur" },
  ],
  year: [
    { required: true, message: "Năm không được để trống", trigger: "blur" },
    { min: 4, max: 4, message: "Năm không hợp lệ", trigger: "blur" },
  ],
});

const router = useRouter();

const submitForm = async (formEl: FormInstance | undefined) => {
  if (!formEl) return;
  await formEl.validate((valid, fields) => {
    if (valid) {
      // Submit form to search
      router.push({
        name: "result",
        query: {
          score: formSearch.score,
          block: formSearch.block,
          year: formSearch.year,
        },
      });
      console.log("submit!");
    } else {
      console.log("error submit!", fields);
    }
  });
};
</script>

<style lang="scss" scoped>
:deep(.el-card__header) {
  border-bottom: 0px;
}

:deep(.el-card__footer) {
  border-top: 0px;
}

.hint-container {
  .el-divider--vertical {
    height: 40px;
  }
}

.quote-container {
}
</style>
