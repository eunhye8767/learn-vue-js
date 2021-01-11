<template>
  <div>
    {{ str }}
    <app-header></app-header>
  </div>
</template>

<script>
import AppHeader from './components/AppHeader.vue'
export default {
  // 인스턴스 속성 옵션 or 컴포넌트 속성
  
  data: function() {
    return {
      str: 'hi'
    }
  },
  components: {
    'app-header': AppHeader
  }

}
</script>

<style>

</style>