<template lang="pug">
  header.header
    .container
      .header-nav
        .header-item
          a(href="https://prodoctorov.ru/kazan/vrach/684097-butorina/") +79274238025
        .header-item-center
          .header-item-name Анастасия Александровна Буторина
          .header-item-proff Врач Педиатр
        .header-item butorina@mail.ru
</template>

<script>
export default {
  name: "header"
}
</script>

<style scoped>
.header{
  height: 100px;
  width: 100%;
  background-color: rgba(212, 131, 194, 60%);
  display: flex;
  justify-content: space-around;
  gap: 30px;
  align-items: center;
  align-content: center;
  font-weight: 800;
}
.header-item{
  min-width: min-content;
}
.header-nav{
  display: flex;
  justify-content: space-around;
}
.header-item-center{
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
