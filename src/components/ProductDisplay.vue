<template>
  <div class="wrapper">
    <div class="card-view" :id="cat" v-if="cat == 'men' || cat == 'women'">
      <div class="image-div">
        <img :src="product.image" />
      </div>
      <div class="product-div">
        <h1 class="product-title">{{ product.title }}</h1>
        <div style="display: flex; justify-content: space-between">
          <h3 style="font-weight: 300">{{ product.category }}</h3>
          <h3 style="font-weight: 300">{{ product.rating.rate }}/5</h3>
        </div>
        <p style="font-weight: 700">{{ product.description }}</p>
        <div
          style="
            display: flex;
            flex-grow: 1;
            flex-direction: column;
            justify-content: end;
            gap: 20px;
          "
        >
          <h2>${{ product.price }}</h2>
          <div style="display: flex; justify-content: space-around; gap: 5px">
            <button style="width: 100%; height: 42px" id="buy">Buy now</button>
            <button
              style="width: 100%; height: 42px"
              id="next"
              @click="nextProduct"
            >
              Next product
            </button>
          </div>
        </div>
      </div>
    </div>
    <div class="card-view" id="unavailable" v-else>
      <h1>This product is unavailable to show</h1>
      <button
        @click="nextProduct"
        style="
          width: 465px;
          height: 42px;
          background-color: transparent;
          border: 3px solid #3f3f3f;
          font-weight: 700;
          font-size: 1rem;
          border-radius: 5px;
          cursor: pointer;
        "
      >
        Next product
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      product: {},
      id: 1,
      cat: "",
    };
  },
  methods: {
    nextProduct() {
      this.id === 20 ? (this.id = 1) : this.id++;
      this.getProduct();
    },
    prevProduct() {
      this.id--;
      this.getProduct();
    },
    async getProduct() {
      await fetch("https://fakestoreapi.com/products/" + this.id)
        .then((res) => res.json())
        .then((data) => {
          this.product = data;
          if (data.category == "men's clothing") {
            this.cat = "men";
          } else if (data.category == "women's clothing") {
            this.cat = "women";
          } else {
            this.cat = "unavailable";
          }
        });
    },
  },
  async created() {
    const product = await fetch("https://fakestoreapi.com/products/1")
      .then((res) => res.json())
      .then((data) => {
        this.product = data;
        if (data.category == "men's clothing") {
          this.cat = "men";
        } else if (data.category == "women's clothing") {
          this.cat = "women";
        } else {
          this.cat = "unavailable";
        }
      });
  },
};
</script>

<style scoped>
.wrapper {
  width: 1034px;
  height: 580px;
  background-color: white;
}
.card-view {
  display: flex;
  justify-content: space-evenly;
  height: 100%;
  box-shadow: 0px 0px 10px #3f3f3f;
  border-radius: 5px;
}
.card-view#unavailable {
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 20px;
}

.image-div {
  align-items: center;
  display: flex;
}
img {
  width: 301px;
  height: 383px;
  object-fit: fill;
  flex-grow: 1;
}
.product-div {
  width: 60%;
  gap: 30px;
  display: flex;
  flex-direction: column;
  margin: 70px 0;
  color: #3f3f3f;
}
.product-div h1 {
  font-size: 2rem;
}

.card-view#men {
  border: 2px solid #002772;
}
#men h1,
#men h2 {
  color: #002772;
}

#men #buy {
  background-color: #002772;
  color: white;
  border: 0;
  font-size: 1rem;
  font-weight: 700;
  cursor: pointer;
  border-radius: 5px;
}
#men #next {
  color: #002772;
  font-size: 1rem;
  font-weight: 700;
  cursor: pointer;
  border: 3px solid #002772;
  background-color: white;
  border-radius: 5px;
}

.card-view#women {
  border: 2px solid #720060;
}
#women h1,
#women h2 {
  color: #720060;
}

#women #buy {
  background-color: #720060;
  color: white;
  border: 0;
  font-size: 1rem;
  font-weight: 700;
  cursor: pointer;
  border-radius: 5px;
}
#women #next {
  color: #720060;
  font-size: 1rem;
  font-weight: 700;
  cursor: pointer;
  border: 3px solid #720060;
  background-color: white;
  border-radius: 5px;
}
</style>
