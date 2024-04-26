<template>
  <div class="table-box"></div>
</template>
<script setup>
import { ref, onMounted } from "vue";
/* 数据  */
/* 方法 */
const addEmoji = (string, target, emoji) => {
  let result = "";
  let index = string.indexOf(target);
  let targetIndex = 0;
  while (index !== -1) {
    result += string.slice(targetIndex, index) + target + emoji;
    targetIndex = index + target.length;
    index = string.indexOf(target, targetIndex);
  }
  result += string.slice(targetIndex);
  return result;
};
onMounted(() => {
  const str = addEmoji("天上有白云，地上有山鸡", "上", "☀");
  console.log(str);
});
</script>

<style scoped>
.table-box {
  margin: 200px auto;
  width: 800px;
}
</style>