<template>
  <div
    class="flex h-full w-full flex-col items-center justify-center px-[12vw]"
  >
    <el-row
      class="w-full space-y-4 md:space-y-0"
      :gutter="20"
      justify="center"
      align="middle"
    >
      <el-col :sm="12" :xs="24">
        <!-- Container -->
        <el-card class="card-container w-full md:w-[26vw]">
          <template #header>
            <!-- header -->
            <div class="block">
              <div class="flex items-center">
                <img src="@/assets/svg/logo.svg" class="w-6" alt="icon" />
                <el-divider direction="vertical" />
                <span class="text-2xl font-bold">{{
                  headerData.title.toUpperCase()
                }}</span>
              </div>
              <p class="text-sm font-light">{{ headerData.description }}</p>
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
                      size="large"
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
                    <el-select
                      v-model="formSearch.block"
                      filterable
                      placeholder="A00"
                      size="large"
                      style="width: 100%"
                      :suffix-icon="searchIcon"
                      :suffix-transition="false"
                    >
                      <el-option-group
                        v-for="group in blockGroup"
                        :key="group.label"
                        :label="group.label"
                      >
                        <el-option
                          v-for="item in group.options"
                          :key="item.value"
                          :label="item.label"
                          :value="item.value"
                        />
                      </el-option-group>
                    </el-select>
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
                    <el-select
                      v-model="formSearch.year"
                      size="large"
                      placeholder="2010"
                    >
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
                    <p class="text-sm font-light">{{ formLabel.hint }}</p>
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
                      style="
                        font-size: 1rem;
                        border: 2px solid #0c1136;
                        border-radius: 0px;
                        background: #e72166;
                        box-shadow: 4px 4px 0px 0px #0c1136;
                      "
                      type="primary"
                      size="large"
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
              <el-divider style="width: 60%; border-color: #0c1136" />
              <p class="text-sm font-light">{{ footerData.title }}</p>
            </div>
          </template>
        </el-card>
      </el-col>
      <el-col :sm="12" :xs="24">
        <!-- Quote -->
        <div class="font-bold">
          <el-text style="color: white; font-size: 2.4rem; line-height: 1">
            {{ "Không học,".toUpperCase() }}<br />
            {{ "Chỉ có bốc cứt".toUpperCase() }}
          </el-text>
          <br />
          <el-text
            class="mx-1"
            style="color: white; font-size: 1rem; line-height: 1"
            >{{ "- Câu này quen mà".toUpperCase() }}</el-text
          >
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

const blockGroup = [
  {
    label: "Khối A (Toán – Lý – Hóa và các tổ hợp liên quan)",
    options: [
      { value: "A00", label: "A00 (Toán, Vật Lý, Hóa Học)" },
      { value: "A01", label: "A01 (Toán, Vật Lý, Tiếng Anh)" },
      { value: "A02", label: "A02 (Toán, Vật Lý, Sinh Học)" },
      { value: "A03", label: "A03 (Toán, Vật Lý, Lịch Sử)" },
      { value: "A04", label: "A04 (Toán, Vật Lý, Địa Lý)" },
      { value: "A05", label: "A05 (Toán, Hóa Học, Lịch Sử)" },
      { value: "A06", label: "A06 (Toán, Hóa Học, Địa Lý)" },
      { value: "A07", label: "A07 (Toán, Lịch Sử, Địa Lý)" },
      { value: "A08", label: "A08 (Toán, Lịch Sử, Giáo Dục Công Dân)" },
      { value: "A09", label: "A09 (Toán, Địa Lý, Giáo Dục Công Dân)" },
      { value: "A10", label: "A10 (Toán, Vật Lý, Giáo Dục Công Dân)" },
      { value: "A11", label: "A11 (Toán, Hóa Học, Giáo Dục Công Dân)" },
      { value: "A12", label: "A12 (Toán, Khoa Học Tự Nhiên, Khoa Học Xã Hội)" },
      { value: "A14", label: "A14 (Toán, Khoa Học Tự Nhiên, Địa Lý)" },
      {
        value: "A15",
        label: "A15 (Toán, Khoa Học Tự Nhiên, Giáo Dục Công Dân)",
      },
      { value: "A16", label: "A16 (Toán, Khoa Học Tự Nhiên, Ngữ Văn)" },
      { value: "A17", label: "A17 (Toán, Khoa Học Xã Hội, Vật Lý)" },
      { value: "A18", label: "A18 (Toán, Khoa Học Xã Hội, Hóa Học)" },
    ],
  },
  {
    label: "Khối B (Toán – Hóa – Sinh và các tổ hợp liên quan)",
    options: [
      { value: "B00", label: "B00 (Toán, Hóa Học, Sinh Học)" },
      { value: "B01", label: "B01 (Toán, Sinh Học, Lịch Sử)" },
      { value: "B02", label: "B02 (Toán, Sinh Học, Địa Lý)" },
      { value: "B03", label: "B03 (Toán, Sinh Học, Ngữ Văn)" },
      { value: "B04", label: "B04 (Toán, Sinh Học, Giáo Dục Công Dân)" },
      { value: "B05", label: "B05 (Toán, Sinh Học, Khoa Học Xã Hội)" },
      { value: "B08", label: "B08 (Toán, Sinh Học, Tiếng Anh)" },
    ],
  },
  {
    label: "Khối C (Văn – Sử – Địa và các tổ hợp liên quan)",
    options: [
      { value: "C00", label: "C00 (Ngữ Văn, Lịch Sử, Địa Lý)" },
      { value: "C01", label: "C01 (Ngữ Văn, Toán, Vật Lý)" },
      { value: "C02", label: "C02 (Ngữ Văn, Toán, Hóa Học)" },
      { value: "C03", label: "C03 (Ngữ Văn, Toán, Lịch Sử)" },
      { value: "C04", label: "C04 (Ngữ Văn, Toán, Địa Lý)" },
      { value: "C05", label: "C05 (Ngữ Văn, Lịch Sử, Hóa Học)" },
      { value: "C06", label: "C06 (Ngữ Văn, Lịch Sử, Sinh Học)" },
      { value: "C07", label: "C07 (Ngữ Văn, Lịch Sử, Giáo Dục Công Dân)" },
      { value: "C08", label: "C08 (Ngữ Văn, Hóa Học, Sinh Học)" },
      { value: "C09", label: "C09 (Ngữ Văn, Vật Lý, Địa Lý)" },
      { value: "C10", label: "C10 (Ngữ Văn, Hóa Học, Giáo Dục Công Dân)" },
      { value: "C12", label: "C12 (Ngữ Văn, Sinh Học, Lịch Sử)" },
      { value: "C13", label: "C13 (Ngữ Văn, Sinh Học, Địa Lý)" },
      { value: "C14", label: "C14 (Ngữ Văn, Toán, Giáo Dục Công Dân)" },
      { value: "C15", label: "C15 (Ngữ Văn, Toán, Khoa Học Xã Hội)" },
      { value: "C16", label: "C16 (Ngữ Văn, Vật Lý, Giáo Dục Công Dân)" },
      { value: "C17", label: "C17 (Ngữ Văn, Địa Lý, Giáo Dục Công Dân)" },
      { value: "C18", label: "C18 (Ngữ Văn, Hóa Học, Giáo Dục Công Dân)" },
      { value: "C19", label: "C19 (Ngữ Văn, Lịch Sử, Giáo Dục Công Dân)" },
      { value: "C20", label: "C20 (Ngữ Văn, Địa Lý, Giáo Dục Công Dân)" },
    ],
  },
  {
    label: "Khối D (Toán – Văn – Ngoại ngữ và các tổ hợp liên quan)",
    options: [
      { value: "D01", label: "D01 (Ngữ Văn, Toán, Tiếng Anh)" },
      { value: "D02", label: "D02 (Ngữ Văn, Toán, Tiếng Nga)" },
      { value: "D03", label: "D03 (Ngữ Văn, Toán, Tiếng Pháp)" },
      { value: "D04", label: "D04 (Ngữ Văn, Toán, Tiếng Trung)" },
      { value: "D05", label: "D05 (Ngữ Văn, Toán, Tiếng Đức)" },
      { value: "D06", label: "D06 (Ngữ Văn, Toán, Tiếng Nhật)" },
      { value: "D07", label: "D07 (Toán, Hóa Học, Tiếng Anh)" },
      { value: "D08", label: "D08 (Toán, Sinh Học, Tiếng Anh)" },
      { value: "D09", label: "D09 (Toán, Lịch Sử, Tiếng Anh)" },
      { value: "D10", label: "D10 (Toán, Địa Lý, Tiếng Anh)" },
      { value: "D11", label: "D11 (Ngữ Văn, Vật Lý, Tiếng Anh)" },
      { value: "D12", label: "D12 (Ngữ Văn, Hóa Học, Tiếng Anh)" },
      { value: "D13", label: "D13 (Ngữ Văn, Sinh Học, Tiếng Anh)" },
      { value: "D14", label: "D14 (Ngữ Văn, Lịch Sử, Tiếng Anh)" },
      { value: "D15", label: "D15 (Ngữ Văn, Địa Lý, Tiếng Anh)" },
      { value: "D66", label: "D66 (Ngữ Văn, Giáo Dục Công Dân, Tiếng Anh)" },
      { value: "D84", label: "D84 (Toán, Giáo Dục Công Dân, Tiếng Anh)" },
    ],
  },
  {
    label: "Khối H (Năng khiếu Mỹ thuật)",
    options: [
      { value: "H00", label: "H00 (Ngữ Văn, Năng Khiếu 1, Năng Khiếu 2)" },
      { value: "H01", label: "H01 (Toán, Ngữ Văn, Vẽ)" },
      { value: "H02", label: "H02 (Ngữ Văn, Hình Họa, Trang Trí Màu)" },
      { value: "H03", label: "H03 (Toán, Khoa Học Tự Nhiên, Vẽ Năng Khiếu)" },
      { value: "H04", label: "H04 (Toán, Tiếng Anh, Vẽ Năng Khiếu)" },
      { value: "H05", label: "H05 (Ngữ Văn, Khoa Học Xã Hội, Vẽ Năng Khiếu)" },
      { value: "H06", label: "H06 (Ngữ Văn, Tiếng Anh, Vẽ Năng Khiếu)" },
    ],
  },
  {
    label: "Khối M (Năng khiếu Sư phạm, Giáo dục Mầm non, Tiểu học)",
    options: [
      {
        value: "M00",
        label: "M00 (Ngữ Văn, Toán, Đọc diễn cảm, Hát)",
      },
      {
        value: "M01",
        label: "M01 (Ngữ Văn, Toán, Năng Khiếu)",
      },
      {
        value: "M02",
        label: "M02 (Ngữ Văn, Khoa Học Tự Nhiên, Năng Khiếu)",
      },
      {
        value: "M03",
        label: "M03 (Ngữ Văn, Khoa Học Xã Hội, Năng Khiếu)",
      },
      {
        value: "M04",
        label: "M04 (Ngữ Văn, Toán, Năng Khiếu Giáo dục Mầm non)",
      },
    ],
  },
  {
    label: "Khối N (Năng khiếu Âm nhạc)",
    options: [
      {
        value: "N00",
        label: "N00 (Ngữ Văn, Năng Khiếu Âm nhạc 1, Năng Khiếu Âm nhạc 2)",
      },
      {
        value: "N01",
        label: "N01 (Ngữ Văn, Năng Khiếu Âm nhạc 1, Năng Khiếu Âm nhạc 2)",
      },
      {
        value: "N02",
        label: "N02 (Ngữ Văn, Năng Khiếu Âm nhạc 1, Năng Khiếu Âm nhạc 2)",
      },
    ],
  },
  {
    label: "Khối R (Năng khiếu Báo chí, Sân khấu, Truyền hình)",
    options: [
      {
        value: "R00",
        label: "R00 (Ngữ Văn, Năng Khiếu Báo chí 1, Năng Khiếu Báo chí 2)",
      },
      {
        value: "R01",
        label: "R01 (Ngữ Văn, Toán, Năng Khiếu Báo chí)",
      },
      {
        value: "R02",
        label: "R02 (Ngữ Văn, Tiếng Anh, Năng Khiếu Báo chí)",
      },
      {
        value: "R03",
        label: "R03 (Ngữ Văn, Lịch Sử, Năng Khiếu Báo chí)",
      },
      {
        value: "R06",
        label: "R06 (Văn, Khoa học tự nhiên, Năng khiếu báo chí)",
      },
      {
        value: "R07",
        label: "R07 (Văn, Toán, Năng khiếu ảnh báo chí)",
      },
      {
        value: "R08",
        label: "R08 (Văn, Anh, Năng khiếu truyền hình)",
      },
    ],
  },
  {
    label: "Khối S (Năng khiếu Sân khấu, Điện ảnh)",
    options: [
      {
        value: "S00",
        label: "S00 (Ngữ Văn, Năng Khiếu Sân khấu, Năng Khiếu Điện ảnh)",
      },
    ],
  },
  {
    label: "Khối T (Năng khiếu Thể dục thể thao)",
    options: [
      {
        value: "T00",
        label: "T00 (Toán, Sinh Học, Năng Khiếu Thể dục thể thao)",
      },
      {
        value: "T01",
        label: "T01 (Toán, Ngữ Văn, Năng Khiếu Thể dục thể thao)",
      },
      {
        value: "T02",
        label: "T02 (Ngữ Văn, Sinh Học, Năng Khiếu Thể dục thể thao)",
      },
      {
        value: "T03",
        label: "T03 (Ngữ Văn, Địa Lý, Năng Khiếu Thể dục thể thao)",
      },
      {
        value: "T04",
        label: "T04 (Ngữ Văn, Giáo dục công dân, Năng Khiếu Thể dục thể thao)",
      },
    ],
  },
  {
    label: "Khối V (Năng khiếu Kiến trúc, Xây dựng)",
    options: [
      {
        value: "V00",
        label: "V00 (Toán, Vật Lý, Vẽ Mỹ thuật)",
      },
      {
        value: "V01",
        label: "V01 (Toán, Ngữ Văn, Vẽ Mỹ thuật)",
      },
      {
        value: "V02",
        label: "V02 (Toán, Tiếng Anh, Vẽ Mỹ thuật)",
      },
      {
        value: "V03",
        label: "V03 (Toán, Vật Lý, Vẽ Hình họa Mỹ thuật)",
      },
      {
        value: "V04",
        label: "V04 (Toán, Ngữ Văn, Vẽ Hình họa Mỹ thuật)",
      },
    ],
  },
  {
    label: "Khối K (Liên thông Cao đẳng lên Đại học)",
    options: [
      {
        value: "K00",
        label: "K00 (Toán, Vật Lý, Hóa Học)",
      },
      {
        value: "K01",
        label: "K01 (Toán, Vật Lý, Sinh Học)",
      },
      {
        value: "K02",
        label: "K02 (Toán, Hóa Học, Sinh Học)",
      },
    ],
  },
];

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

:deep(.el-form-item__label) {
  font-size: 0.75rem;
  font-weight: 400;
}

.hint-container {
  .el-divider--vertical {
    height: 40px;
  }
}

:deep(.el-select__wrapper) {
  border-radius: 0px;
  border: 1px solid #0c1136;
  background-color: #e9e7e7;
}

:deep(.el-input__wrapper) {
  border-radius: 0px;
  border: 1px solid #0c1136;
  background-color: #e9e7e7;
}

:deep(.el-select__caret.is-reverse) {
  transform: none;
}

.card-container {
  @apply rounded-none border-[2px] border-solid border-[#0C1136];
  box-shadow: 8px 8px 0px 0px #0c1136;
}
</style>
