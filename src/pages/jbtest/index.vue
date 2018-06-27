<template>
  <div class="container p-0 bg-light" style="min-height: 100vh">
    <div class="row  p-2 justify-content-center align-items-center">
      <a class="d-block btn text-white" :href="'/pages/jbdashboard/main'" role="button" style="background-color: #D78F30">
        Start a New Ventures1
      </a>
    </div>

    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text" id="basic-addon1">@</span>
      </div>
      <input type="text" class="form-control" placeholder="Username" aria-label="Username" aria-describedby="basic-addon1">
    </div>

    <div class="input-group mb-3">
      <div class="input-group-prepend">
        <span class="input-group-text">$</span>
      </div>
      <input type="text" class="form-control" aria-label="Amount (to the nearest dollar)">
      <div class="input-group-append">
        <span class="input-group-text">.00</span>
      </div>
    </div>

    <div class="input-group">
      <div class="input-group-prepend">
        <span class="input-group-text">With textarea</span>
      </div>
      <textarea class="form-control" aria-label="With textarea"></textarea>
    </div>

    <view class="section section_gap">
      <text class="section__title">设置step</text>
      <view class="body-view">
        <slider bindchange="slider2change" step="5"/>
      </view>
    </view>

  </div>

</template>

<script>


</script>

<style>


</style>
