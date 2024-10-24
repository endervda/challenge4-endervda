<script setup>
    let name = 'User';
    let image = 'src/assets/profilepicture.png';
    const props = defineProps({
        title: String });
</script>

<template>
    <div class="video__detail">
        <img :src="image" alt="profile picture">
        <a href="" class="username">
            <p>{{ name }}</p>
        </a>
    </div>
            <h2>{{ title }}</h2>
</template>

<style scoped>

.video__detail {
  display: flex;
  align-items: center;
  gap: 10px;
  margin-bottom: 20px;
  background-color: #ff3b5c;
  padding: 10px;
  justify-content: flex-start; 
  padding-left: 20px; 
}

img {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  object-fit: cover;
  border: 2px solid #ffffff;
  background-color: #000000;
}

.username {
  text-decoration: none;
}

.username p {
  font-size: 16px;
  font-weight: bold;
  color: #ffffff;
  margin: 0;
}

h2 {
  font-size: 24px;
  font-weight: bold;
  color: #333;
  margin-left: 30px; 
  
}

</style>
