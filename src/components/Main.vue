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
                .price-item__text  Интерпритация анализов,рекомендации по исследованиям
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
  main{
    background-color: rgba(255, 245, 252, 50%);
  }
  .main{
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin: 0 20px;
    font-weight: 600;
  }
  .block{
    padding-top: 20px;
  }
  .line{
    padding: 10px 0;
    border-bottom: 1px solid #d483c299;;
  }
  .salutation {
    display: flex;
    justify-content: center;
    align-content: center;
    flex-wrap: wrap;
    gap: 20px;
  }

  .salutation-photo{
    flex-grow: 1;
    flex-basis: 0;
    width: 500px;
    overflow: hidden;
    border-radius: 427px;
    border: 3px solid #eecbe5;
    display: flex;
    max-width: 300px;
  }
  .salutation-text{
    flex-grow: 1;
    flex-basis: 0;
    padding: 23px 19px;
    max-width: 100%;
    font-size: 20px;
    border: 3px solid #eecbe5;
    border-radius: 29px;
    margin-top: 100px;
    height: min-content;

  }
  .services-title{
    font-size: 32px;
  }
  .services-list{
  }
  .services-item{
    padding-top: 10px;
  }
  .price-title{
    font-size: 32px;
  }
  .price-list{
  }
  .price-item{
    padding-top: 10px;
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    width: 100%;
  }
  .price-item__text{
    width: max-content;
  }
  .price-item__underline{
    margin: 0 10px;
    flex-grow: 1;
    border-bottom: 1px solid black;
  }
  .price-item__cell{

  }
  .price-item__button{
    margin-left: 20px;
  }
  .image-container {
    position: relative;
    width: 100%;
    padding-bottom: 75%; /* 4:3 aspect ratio */
  }
  .image-container img {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    object-fit: cover;
    object-position: center;
    max-width: 100%;
    max-height: 100%;
  }
</style>
