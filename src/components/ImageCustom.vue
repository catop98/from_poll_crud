


<template>
<div class="image_numb">
	<label class="box">
  <input type="radio" name="test" value="small" checked>
  <img src="https://via.placeholder.com/40x60/871a89/fff&text=1">
</label>


<label class="box">
  <input type="radio" name="test" value="big">
  <img src="https://via.placeholder.com/40x60/871a89/fff&text=2">
</label>

<label class="box">
  <input type="radio" name="test" value="big">
  <img src="https://via.placeholder.com/40x60/871a89/fff&text=3">
</label>

<label class="box">
  <input type="radio" name="test" value="big">
  <img src="https://via.placeholder.com/40x60/871a89/fff&text=4">
</label>

<label class="box">
  <input type="radio" name="test" value="big">
  <img src="https://via.placeholder.com/40x60/871a89/fff&text=5">
</label>

<label class="box">
  <input type="radio" name="test" value="big">
  <img src="https://via.placeholder.com/40x60/871a89/fff&text=6">
</label>

<label class="box">
  <input type="radio" name="test" value="big">
  <img src="https://via.placeholder.com/40x60/871a89/fff&text=7">
</label>
</div>
	
</template>

<style>

.box{
  padding: 10px;
  
}


/* HIDE RADIO */
[type=radio] { 
  position: absolute;
  opacity: 0;
  width: 0;
  height: 0;
  padding: 90px;
}

/* IMAGE STYLES */
[type=radio] + img {
  cursor: pointer;
 
  
}

/* CHECKED STYLES */
[type=radio]:checked + img {
  outline: 4px solid #e11d7a;
 
}
</style>