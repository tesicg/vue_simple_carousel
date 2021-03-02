<template>
  <div class="carousel">
    <div class="container">
      <div class="row">
        <div class="col-5 carousel__left">
          <img class="carousel__cursor-pointer" src="../assets/chevron-left.svg" @click="onPrevious"/>
        </div>
        <div class="col-2 carousel__middle">
          {{ subArrayValues }}
        </div>
        <div class="col-5 carousel__right">
          <img class="carousel__cursor-pointer" src="../assets/chevron-right.svg" @click="onNext"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
function* subArrayGenerator(inputArray, subArrayLength = 3) {
  let prev = false
  for (let i = 0; i < inputArray.length;) {
    if (prev) {
      const diff = i - (subArrayLength - 3) * 2
      i = diff < 0 ? inputArray.length + diff : diff
    }
    const subArray = []
    for (let j = 0; j < subArrayLength; j++) {
      subArray.push(inputArray[i])
      i = i < inputArray.length - 1 ? i + 1 : 0
    }
    prev = (yield subArray) === false
  }
}

export default {
  data() {
    return {
      input: [1, 2, 3, 4, 5],
      subArrayIterator: {},
      subArrayValues: {}
    }
  },
  mounted() {
    this.subArrayIterator = subArrayGenerator(this.input)
    this.subArrayValues = this.subArrayIterator.next().value
  },
  methods: {
    onNext () {
      this.subArrayValues = this.subArrayIterator.next().value
    },
    onPrevious () {
      this.subArrayValues = this.subArrayIterator.next(false).value
    }
  }
}
</script>

<style scoped>
.carousel {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 100vh;
}
.carousel__cursor-pointer {
  cursor: pointer;
}
.carousel__left {
  display: flex;
  justify-content: flex-end;
}
.carousel__middle {
  text-align: center;
}
.carousel__right {
  display: flex;
  justify-content: flex-start;
}
</style>