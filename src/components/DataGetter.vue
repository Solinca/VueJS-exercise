<template>
  <div class="data">
    <div class="totalContainer">
      <span>{{ totalPrice }} $ {{ stashedElements }}</span>
    </div>
    <div v-for="food in foods" :key="food.name" class="foodItem" v-bind:class="{ active: active === food }"
         v-on:click.self="active = active === food ? null : food" v-bind:style="{backgroundColor: food.color}">
      {{ food.name }}
      <div class="subFoodItemContainer">
        <div v-for="item in food.items" :key="item.name" v-on:click="addItem(item)" class="subFoodItem">
          {{ item.name }} {{ item.price }}$
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'DataGetter',
  data () {
    return {
      stashedElementsArray: [],
      stashedElements: '',
      totalPrice: 0,
      active: null,
      foods: null
    }
  },
  mounted () {
    this.$http
      .get('./data/data.json')
      .then(response => {
        try {
          let result = []
          let foods = response.data.food
          for (let key in foods) {
            if (foods.hasOwnProperty(key)) {
              result.push(foods[key])
            }
          }
          this.foods = result
        } catch {
          this.foods = []
        }
      })
  },
  methods: {
    addItem: function (item) {
      this.totalPrice += item.price
      this.stashedElementsArray.push(item.name)
      this.stashedElements = '( ' + this.stashedElementsArray.join(' + ') + ' )'
    }
  }
}
</script>

<style scoped lang="scss">
  .totalContainer {
    width: 800px;
    margin: auto;
    text-align: left;
    font-size: 40px;
  }

  .foodItem, .subFoodItem {
    position: relative;
    display: inline-block;
    width: 250px;
    height: 100px;
    margin: 20px;
    border: 1px solid rgba(black, 0.5);
    border-radius: 20px;
    vertical-align: middle;
    line-height: 100px;
    font-weight: bold;
    text-transform: uppercase;
    text-shadow: 1px 1px 1px black;
    color: white;
    cursor: pointer;
  }

  .foodItem.active {
    .subFoodItemContainer {
      top: 120px;
    }

    @for $i from 1 through 10 {
      .subFoodItem:nth-child(#{$i}) {
        top: 70px * ($i - 1);
      }
    }
  }

  .subFoodItemContainer {
    position: absolute;
    top: 0;
    right: 0;
    width: 250px;
    transition: top 500ms;
    z-index: -1;
  }

  .subFoodItem {
    position: absolute;
    top: 0;
    right: 50px;
    display: block;
    width: 150px;
    height: 50px;
    margin: 0 auto;
    background-color: darkblue;
    line-height: 50px;
    transition: top 500ms;
  }
</style>
