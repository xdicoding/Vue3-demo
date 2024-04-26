<template>
  <div class="table-box">
    <el-input v-model="value" />
    <div class="img-box">
      <img
        v-for="img in filterImgs"
        :src="img.src"
        :key="img.name"
        width="100"
      />
    </div>
  </div>
</template>
<script setup>
import { ref, onMounted, computed } from "vue";
import Tesseract from "tesseract.js";
/* 数据  */
const value = ref("");
const allImgs = ref([]);
const filterImgs = computed(() =>
  allImgs.value.filter(({ keyword }) => keyword.includes(value.value))
);
/* 方法 */
const requestImgs = () => {
  //本地图片需放在根目录public，使用localhost访问
  return [
    "https://img2.baidu.com/it/u=757556849,1407103140&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=500",
    "https://k.sinaimg.cn/n/sinakd20122/361/w1125h2436/20240301/790c-758c8e0ea010058dda37dffb6c47912d.jpg/w700d1q75cms.jpg",
    "https://img0.baidu.com/it/u=1831487838,4286702861&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=889",
    "https://img2.baidu.com/it/u=166824242,3279274137&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=889",
    "https://img2.baidu.com/it/u=2229817200,2743896509&fm=253&fmt=auto&app=138&f=JPEG?w=500&h=889",
  ].map((img) => ({
    name: img,
    src: img,
  }));
};

const resolveImg = (obj) => {
  const o = { ...obj };
  return Tesseract.recognize(o.src, "chi_sim") //英文:eng   中文:chi_sim   中英文:eng+chi_sim
    .then(({ data: { text } }) => {
      return { ...o, keyword: text };
    })
    .catch(() => {
      return { ...o, keyword: o.src };
    });
};

const getImgs = async () => {
  const res = requestImgs();
  allImgs.value = await Promise.all(res.map((o) => resolveImg(o)));
  console.log(allImgs.value);
};

onMounted(() => {
  getImgs();
});
</script>

<style scoped>
.table-box {
  margin: 200px auto;
  width: 800px;
}
.img-box {
  margin-top: 20px;
  display: flex;
  justify-content: center;
}
</style>