<script setup lang="ts">
const format = ref(0)
const imgShow = ref()
const inputimg = ref()

function start() {
  getImg((image: HTMLImageElement) => {
    const can = imgToCanvas(image)
    imgShow.value.setAttribute('src', canvasToImg(can))
  })
}
function imgToCanvas(image: HTMLImageElement) {
  const canvas = document.createElement('canvas')
  canvas.width = image.width
  canvas.height = image.height
  canvas.getContext('2d')?.drawImage(image, 0, 0)

  return canvas
}
// 把image 转换为 canvas对象

// canvas转换为image
function canvasToImg(canvas: HTMLCanvasElement) {
  const array = ['image/webp', 'image/jpeg', 'image/png']
  const src = canvas.toDataURL(array[format.value])
  return src
}
// 获取图片信息
function getImg(fn: (image: HTMLImageElement) => void) {
  const imgFile = new FileReader()
  try {
    imgFile.onload = function (e: ProgressEvent<FileReader>) {
      const image: HTMLImageElement = new Image()
      image.src = (e?.target?.result as string)
      image.onload = function () {
        fn(image)
      }
    }
    imgFile.readAsDataURL(inputimg.value.files[0])
  }
  catch (e) {
    console.log(`请上传图片！${e}`)
  }
}
</script>

<template>
  <div max-w="900px" ma>
    <h2 text-2xl text-center>
      图片在线webp/png/jpeg格式转换工具
    </h2>
    <div mt-2>
      <span>选择图片：</span><input ref="inputimg" type="file">
      <div mt-2>
        <span>选择格式：</span>
        <select v-model="format">
          <option :value="0">
            webp格式
          </option>
          <option :value="1">
            jpeg格式
          </option>
          <option :value="2">
            png格式
          </option>
        </select>
      </div>

      <button mt-2 btn @click="start">
        开始转换
      </button>
    </div>
    <div mt-2>
      <p>预览：</p>
      <img ref="imgShow" src="" alt="">
    </div>
    <div mt-2>
      <h3>备注：</h3>
      <p>1、转换后的图片请选择右键保存！</p>
      <p>2、该工具目前仅支持转换为webp、jpeg、png的格式。如果是gif动态图片转换后不保留动态效果。</p>
      <p>3、请使用高版本浏览器,推荐使用Chrome。</p>
    </div>
  </div>
</template>

<route lang="yaml">
meta:
  layout: home
</route>

<style>

</style>
