<template>
  <div>
    {{ str }}
    <!-- app-header v-bind:"(하위컴포넌트에서 정의한)프롭스속성이름="상위컴포넌트의 데이터 이름'></app-header -->
    <app-header 
      v-bind:propsdata="message"
      v-on:renew="renewMessage"></app-header>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue'
export default {
  // 인스턴스 속성 옵션 or 컴포넌트 속성
  
  data: function() {
    return {
      str: 'hi',
      message: 'header',
    }
  },
  components: {
    'app-header': AppHeader
  },
  methods: {
    renewMessage: function() {
      this.message = 'hi';
    }
  }
}
</script>

<style>

</style>