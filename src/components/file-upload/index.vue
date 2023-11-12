<template>
  <main>
    <section class="file-container">
      <div class="file-wrapper" @click="onFileClick">
        <h2>upload file</h2>
      </div>
      <div class="image-wrapper" ref="imageWrapper">
        <div class="image-preview" v-for="(imgUrl, index) in imgUrls" :key="index">
          <img width="100" height="100" loading="lazy" :src="imgUrl" alt="photo">
          <div class="overlay">
            <button @click="onRemove(index)" class="remove-file">x</button>
          </div>
        </div>
      </div>
    </section>
    <input @change="onFileSelect" :multiple="multiple" type="file" hidden ref="file_input" :accept="accept">
  </main>
</template>

<script>
export default {
  name: "FileUpload",
  props: {
    accept: {
      type: String,
      default: "image/png, image/jpeg",
    },
    multiple: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      files: []
    }
  },
  methods: {
    onRemove(index) {
      this.$delete(this.files, index)
    },
    onFileClick() {
      this.$refs.file_input.value = ''
      this.$refs.file_input.click()
    },
    onFileSelect(event) {
      this.files = Array.from(event.target.files)
      this.$emit('onFileSelect', this.files)
      this.$nextTick(() => this.scrollToEnd())
    },
    scrollToEnd() {
      this.$refs.imageWrapper.scrollLeft = this.$refs.imageWrapper.scrollWidth;
    }
  },
  computed: {
    imgUrls() {
      return this.files.map(file => URL.createObjectURL(file))
    }
  }
}
</script>

<style scoped>
.file-container {
  position: relative;
}

.file-wrapper {
  position: relative;
  height: 150px;
  border: 1px dashed #444;
  border-radius: 5px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

.image-wrapper {
  max-width: 100%;
  overflow-x: auto;
  display: flex;
  gap: 10px;
  justify-content: flex-start;
  align-items: center;
  margin-top: 10px;
  white-space: nowrap;
  scrollbar-width: none;
  -ms-overflow-style: none;
  padding-inline: 10px;

}

.image-wrapper .image-preview {
  position: relative;
  transition: all 1s ease;
  overflow: hidden;
}

.image-wrapper img {
  border-radius: 8px;
  max-width: 100px;
  max-height: 100px;
}

.remove-file {
  position: absolute;
  width: 20px;
  height: 20px;
  outline: none;
  border: none;
  border-radius: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: red;
  color: white;
  top: 5px;
  left: 5px;
  cursor: pointer;
}

.overlay {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: rgba(000, 000, 000, .5);
  opacity: 0;
  z-index: 2;
  border-radius: 8px;
  transition: all .6s ease;
}

.image-wrapper .image-preview:hover .overlay {
  opacity: 1;
}

/* Hide the scrollbar (for WebKit browsers) */
/* .image-wrapper::-webkit-scrollbar {
  display: none;
} */
</style>
