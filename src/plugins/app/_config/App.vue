<template>
	<router-view></router-view>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

a {
  color: inherit; 
  text-decoration: none; 
}

a:hover {
  color: red; 
}

a:visited {
  color: gray; 
}

body {
  font-family: 'Arial', sans-serif;
  margin: 0;
  padding: 20px;
  background-color: #f9f9f9;
  color: #333;
  line-height: 1.6;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  background: #ffffff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}


h1 {
  font-size: 24px;
  color: #2c3e50;
  text-align: center;
  margin-bottom: 10px;
}

h2 {
  font-size: 18px;
  color: #34495e;
  text-align: center;
  margin-bottom: 20px;
}

ul {
  list-style-type: none;
  padding: 0;
}

ul li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px 15px;
  border-bottom: 1px solid #ddd;
}

ul li:last-child {
  border-bottom: none;
}

ul li span {
  font-weight: bold;
  color: #2c3e50;
}

input[type="checkbox"] {
  transform: scale(1.2);
  cursor: pointer;
}

ul li:hover {
  background-color: #f4f4f4;
}

button {
  background-color: #3498db; 
  color: #ffffff; 
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  font-size: 16px;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s ease, transform 0.2s ease;
}

button:hover {
  background-color: #2980b9; 
  transform: scale(1.05); 
}

button:active {
  background-color: #1c6ea4; 
  transform: scale(0.98); 
}

button:disabled {
  background-color: #bdc3c7; 
  cursor: not-allowed;
}

input[type="text"] {
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  border: 1px solid #ccc;
  border-radius: 5px;
  font-size: 16px;
  box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.1);
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
}

input[type="text"]:focus {
  border-color: #3498db;
  box-shadow: 0 0 5px rgba(52, 152, 219, 0.5);
  outline: none;
}
</style>
