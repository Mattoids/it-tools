<script setup lang="ts">
import { compareSync, hashSync } from 'bcryptjs';
import { useThemeVars } from 'naive-ui';
import { useCopy } from '@/composable/copy';

const themeVars = useThemeVars();

const input = ref('');
const saltCount = ref(10);
const hashed = computed(() => hashSync(input.value, saltCount.value));
const { copy } = useCopy({ source: hashed, text: 'Hashed string copied to the clipboard' });
const { t } = useI18n();

const compareString = ref('');
const compareHash = ref('');
const compareMatch = computed(() => compareSync(compareString.value, compareHash.value));
</script>

<template>
  <c-card title="Hash">
    <c-input-text
        v-model:value="input"
        :placeholder="t('tools.bcrypt.bcrypt-string-placeholder')"
        raw-text
        :label="t('tools.bcrypt.lable-string') "
        label-position="left"
        label-align="right"
        label-width="120px"
        mb-2
    />
    <n-form-item :label="t('tools.bcrypt.salt') " label-placement="left" label-width="120">
      <n-input-number v-model:value="saltCount" placeholder="Salt rounds..." :max="10" :min="0" w-full />
    </n-form-item>

    <c-input-text :value="hashed" readonly text-center />

    <div mt-5 flex justify-center>
      <c-button @click="copy()">
        {{ t('tools.bcrypt.copy') }}
      </c-button>
    </div>
  </c-card>

  <c-card :title="t('tools.bcrypt.titel-2')">
    <n-form label-width="120">
      <n-form-item :label="t('tools.bcrypt.lable-string')" label-placement="left">
        <c-input-text v-model:value="compareString" :placeholder="t('tools.bcrypt.compare-string-placeholder')" raw-text />
      </n-form-item>
      <n-form-item :label="t('tools.bcrypt.lable-hase')" label-placement="left">
        <c-input-text v-model:value="compareHash" :placeholder="t('tools.bcrypt.compare-hase-placeholder')" raw-text />
      </n-form-item>
      <n-form-item :label="t('tools.bcrypt.compare')" label-placement="left" :show-feedback="false">
        <div class="compare-result" :class="{ positive: compareMatch }">
          {{ compareMatch ? t('tools.bcrypt.compare-yes') : t('tools.bcrypt.compare-yes') }}
        </div>
      </n-form-item>
    </n-form>
  </c-card>
</template>

<style lang="less" scoped>
.compare-result {
  color: v-bind('themeVars.errorColor');

  &.positive {
    color: v-bind('themeVars.successColor');
  }
}
</style>
