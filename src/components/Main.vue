<template lang="pug">
  main
    .main
      .container
        section.block
          .salutation
            .salutation-text  Я - врач-педиатр, Буторина Анастасия Александровна. <br> Я придерживаюсь доказательных и эффективных методов лечения. Окончила Кировский медицинский университет.<br> Я - мама замечательного сына. И на своём опыте знаю, что материнство - это большое счастье, но также оно требует каждодневного огромного труда!
            .salutation-photo
              .image-container
                img(
                  src='@/assets/image/photo.jpg'
                )
        .line
        section.block
          .services
            .services-title С чем я могу вам помочь?
            ul.services-list
              li.services-item - Ребёнок заболел (осмотр на приеме/на дому)
              li.services-item - Уход за новорожденным ребёнком
              li.services-item - Проконсультировать по анализам и исследованиям
              li.services-item - У вас есть вопросы по питанию(прикорм/малоежка/диетотерапия)
              li.services-item - Вопросы по прививкам(составлю индивидуальный график вакцинации/если есть сомнения,подберу нужную научно -доказательную литературу)
              li.services-item - Проблемы со стулом у ребёнка (запор, диарея) ‌колики или беспокоит ,что ребёнок часто срыгивает
              li.services-item - Профилактический осмотр ребёнка ,оценка физического развития ,выдача справок о состоянии здоровья ,в спорт школу или кружки
        .line
        section.block
          .price
            .price-title Стоимость
            ul.price-list
              li.price-item
                .price-item__text  Онлайн — консультация разовая 40 мин(аудио,чат или видео)
                .price-item__underline
                .price-item__cell 1000 руб.
                .price-item__button
                  v-button(
                    mode="pink"
                  ) Записаться

              li.price-item
                .price-item__text  Онлайн — консультация с последующим сопровождением 7 дней
                .price-item__underline
                .price-item__cell 1800 руб.
                .price-item__button
                  v-button(
                    mode="pink"
                  ) Записаться
              li.price-item
                .price-item__text  Интерпритация анализов, рекомендации по исследованиям
                .price-item__underline
                .price-item__cell 400 руб.
                .price-item__button
                  v-button(
                    mode="pink"
                  ) Записаться
              li.price-item
                .price-item__text  Выезд на дом Ново-Савиновский район(остальные по договаренности)
                .price-item__underline
                .price-item__cell 1500 руб.
                .price-item__button
                  v-button(
                    mode="pink"
                  ) Записаться


</template>

<script>
import VButton from "@/components/ui-kit/v-button";
export default {
  name: "Main",
  components: {VButton}
}
</script>
<style lang="scss">

</style>
