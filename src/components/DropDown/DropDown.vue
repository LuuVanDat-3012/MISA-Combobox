<template>
  <div class="gender-container">
    <div class="box-gender" :class="{error : isError}">
      <div class="box-gender-input"  @keyup.38="goUp()" @keyup.40 = "goDown()"  @keyup.enter="confirm()">
        <input
          type="text"
          placeholder=""
          v-model="filter"
          @keyup="FindGender()"
          @click="ActiveContent"
        />
      </div>
      <div class="box-gender-icon" @click="ActiveContent"></div>
    </div>
    <div class="gender-content" v-bind:class="{isActiveContent : isActiveContent}">
      <div
        class="gender"
        v-for="(gender, index) in genders"
        :key="index"
        @click="ChooseGender(gender.value, gender.name)"
        v-bind:class="[BindClass(gender), { isActived: isActive }]"
      >
        <div class="gender-content-text">{{ gender.name }}</div>
      </div>
    </div>
  </div>
</template>
<script>
import $ from 'jquery'
export default {
  name: 'DropDownGender',
  data () {
    return {
      genders: [
        { value: 0, name: 'Nữ' },
        { value: 1, name: 'Nam' },
        { value: 2, name: 'Khác' }
      ],
      genderSelected: {
        value: 0,
        name: 'Nữ'
      },
      isActive: false,
      filter: '',
      isActiveContent: false,
      isError: false, // lỗi
      isChoose: false, // gender được chọn
      indexHover: 0
    }
  },
  methods: {
    FindGender () {
      // Giới tính được chọn sẽ có background xanh
      var keyword = this.filter.charAt(0).toUpperCase() + this.filter.slice(1)
      var count = 0
      if (this.filter === '' || typeof this.filter !== 'undefined') {
        this.genders.forEach(element => {
          $('.gender' + element.value).removeClass('valueFound', 'valueNotFound')
        })
      }
      this.genders.forEach((element) => {
        var index = element.value
        this.isActiveContent = true
        if (element.name.indexOf(keyword) !== -1) {
          $('.gender' + index).removeClass('valueNotFound')
          $('.gender' + index).addClass('valueFound')
        } else {
          $('.gender' + index).removeClass('valueFound')
          $('.gender' + index).addClass('valueNotFound')
          count++
        }
      })
      if (count === this.genders.length) {
        this.isError = true
        this.isActiveContent = false
      } else {
        this.isError = false
      }
    },
    SelectGender () {

    },
    ChooseGender (val, name) {
      this.genderSelected.value = val
      this.genderSelected.name = name
      this.indexHover = val
      this.filter = name
      this.isActiveContent = false
      this.isError = false
      this.genders.forEach(element => {
        var index = element.value
        if (element.value === this.genderSelected.value) {
          $('.gender' + index).addClass('isChoosed')
        } else {
          $('.gender' + index).removeClass('isChoosed')
        }
      })
    },
    BindClass (val) {
      return 'gender' + val.value
    },
    ActiveContent () {
      this.isActiveContent = !this.isActiveContent
      for (let index = 0; index < this.genders.length; index++) {
        if (this.indexHover !== index) {
          $('.gender' + index).removeClass('isChoosed')
        }
      }
      $('.gender' + this.indexHover).addClass('isChoosed')
      $('.gender' + this.indexHover + ' .icon-selected').addClass('isChoosedIcon')
    },

    goUp () {
      if (this.indexHover === 0) {
        $('.gender' + this.indexHover).removeClass('isHover')
        this.indexHover = this.genders.length - 1
      } else {
        $('.gender' + this.indexHover).removeClass('isHover')
        this.indexHover--
      }
      $('.gender' + this.indexHover).addClass('isHover')
    },

    goDown () {
      if (this.indexHover === this.genders.length - 1) {
        $('.gender' + this.indexHover).removeClass('isHover')
        this.indexHover = 0
      } else {
        $('.gender' + this.indexHover).removeClass('isHover')
        this.indexHover++
      }
      $('.gender' + this.indexHover).addClass('isHover')
    },
    // Xác nhận chọn
    confirm () {
      console.log('Enter')
      this.genderSelected.value = this.genders[this.indexHover].value
      this.genderSelected.name = this.genders[this.indexHover].name
      this.filter = this.genders[this.indexHover].name
      this.isActiveContent = false
      $('.gender' + this.indexHover + ' .icon-selected').addClass('isChoosedIcon')
    },
    removeHover () {
      for (let index = 0; index < this.genders.length; index++) {
        $('.gender' + index).removeClass('isHover')
      }
    }

  },
  mounted () {
    this.genders.forEach(element => {
      var index = element.value
      if (element.name.indexOf(this.genderSelected.name) !== -1) {
        $('.gender' + index).removeClass('isChoosed')
      }
    })
  },
  watch: {
    // whenever question changes, this function will run
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...'
      this.debouncedGetAnswer()
    }
  }
}
</script>
<style scoped>
@import url("./DropDown.css");
</style>
