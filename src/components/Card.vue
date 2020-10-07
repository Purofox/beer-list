<template>
  <div class="card">
    <div class="card__img">
      <img :src="data.image_url" alt="">
    </div>
    <div class="card__name">{{ data.id }} {{ data.name }}</div>
    <div class="card__description">{{ data.description }}</div>
    <div class="card__brewers_tips">{{ data.brewers_tips }}</div>

    <div class="card-footer">
      <button class="edit" @click="editEvent">Edit</button>
      <button class="delete" @click="deleteEvent">Delete</button>
    </div>
  </div>
</template>

<script>

export default {
  name: "Card",
  props: {
    data: Object,
    index: Number
  },
  methods: {
    deleteEvent () {
      // eslint-disable-next-line
      this.$emit('deleteEvent', this.index);
    },

    editEvent () {
      // eslint-disable-next-line
      this.$emit('editEvent', this.index);
    }
  }
}

</script>

<style scoped>
  .card {
    box-shadow: 0 0 87.5px rgba(46,56,104,.25);
    display: flex;
    flex-direction: column;
    padding: 10px 20px 40px;
  }

  .card__img {
    height: 350px;
    text-align: center;
    width: 100%;
  }

  .card__img img {
    max-width: 100%;
    height: 100%;
  }

  .card__name {
    font-size: 18px;
    font-weight: bold;
    line-height: 150%;
    padding: 1.042vw 1.875vw 1.25vw 1.198vw;
  }

  .card__description {
    margin-bottom: 10px;
  }

  .card__brewers_tips {
    font-style: italic;
    margin-bottom: 20px;
  }

  .card-footer {
    margin-top: auto;
  }

  button {
    cursor: pointer;
    margin-right: 10px;
  }
</style>