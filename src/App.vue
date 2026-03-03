<template>
  <div class="app">
    <h1>随机菜谱转盘</h1>
    <p>解决你不知道该吃什么的烦恼 qwq</p>
    <button @click="spinRecipe">开始转盘</button>
    <div v-if="currentRecipe" class="recipe-result">
      <h2>今日推荐：{{ currentRecipe.name }}</h2>
      <p>{{ currentRecipe.description }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  data() {
    return {
      recipes: [
        { name: '番茄炒蛋', description: '简单又美味的家常菜' },
        { name: '红烧肉', description: '经典的中式菜肴' },
        { name: '麻婆豆腐', description: '麻辣鲜香的川菜' },
        { name: '清炒时蔬', description: '健康清淡的素菜' },
        { name: '宫保鸡丁', description: '酸甜可口的经典菜' }
      ],
      currentRecipe: null
    }
  },
  methods: {
    spinRecipe() {
      const randomIndex = Math.floor(Math.random() * this.recipes.length)
      this.currentRecipe = this.recipes[randomIndex]
    }
  }
}
</script>

<style>
.app {
  text-align: center;
  padding: 2rem;
  font-family: Arial, sans-serif;
}
.recipe-result {
  margin-top: 2rem;
  padding: 1rem;
  border: 1px solid #ddd;
  border-radius: 8px;
  background-color: #f9f9f9;
}
button {
  padding: 10px 20px;
  font-size: 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
button:hover {
  background-color: #45a049;
}
</style>
