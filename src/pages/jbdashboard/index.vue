<template>
  <div class="container p-0 bg-light" style="min-height: 100vh">
    <!--section 1 -->
    <div class="row  pt-2 justify-content-center align-items-center">
        <h5>鹊山湖项目详情</h5>
    </div>

    <div class="row">
      <div class="col-md-4 block">
        <div class="circle">
          <p>10%</p>
        </div>
        <p>项目总进度为10%</p>
      </div>
    </div>

    <!--section 2 -->
    <div class="row  pt-2 justify-content-center align-items-center">
      <h6>项目总进度表</h6>
    </div>
    <a  :href="'/pages/jbdetail/main'">
      <div class="row p-1">
          <div class="col-4 text-right align-items-center">签订及复核文件</div>
          <div class="col-6 p-2 bg-secondary text-white"></div>
      </div>
    </a>

    <div class="row p-1">
      <div class="col-4 text-right align-items-center">材料确认</div>
      <div class="col-6 p-2 bg-secondary text-white"></div>
    </div>

    <div class="row p-1">
      <div class="col-4 text-right align-items-center">现场符合</div>
      <div class="col-6 p-2 bg-secondary text-white"></div>
    </div>

    <div class="row p-1">
      <div class="col-4 text-right align-items-center">深化设计及图纸</div>
      <div class="col-6 p-2 bg-secondary text-white"></div>
    </div>

    <div class="row p-1">
      <div class="col-4 text-right align-items-center">定制材料备料</div>
      <div class="col-6 p-2 bg-secondary text-white"></div>
    </div>

    <div class="row p-1">
      <div class="col-4 text-right align-items-center">白胚</div>
      <div class="col-6 p-2 bg-secondary text-white"></div>
    </div>

    <div class="row p-1">
      <div class="col-4 text-right align-items-center">产品运输</div>
      <div class="col-6 p-2 bg-secondary text-white"></div>
    </div>

    <div class="row p-1">
      <div class="col-4 text-right align-items-center">产品现场安装</div>
      <div class="col-6 p-2 bg-secondary text-white"></div>
    </div>

    <div class="row p-1">
      <div class="col-4 text-right align-items-center">竣工验收</div>
      <div class="col-6 p-2 bg-secondary text-white"></div>
    </div>

    <div class="row p-1">
      <div class="col-4 text-right align-items-center">售后服务</div>
      <div class="col-6 p-2 bg-secondary text-white"></div>
    </div>

    <!--section 3 -->
     <div class="row p-3 justify-content-center align-items-center">
      <h6>当前进度</h6>
      <div class="col-md-4 block">
        <div class="circle">
          <p>50%</p>
        </div>
        <p>当前进度为材料确认</p>
      </div>
    </div>

  </div>

</template>

<script>


</script>

<style>
  .block {
    border: 0px solid #95BBDD;
    text-align: center;
    vertical-align: middle;
  }
  .circle {
    background: #95BBDD;
    border-radius: 95px;
    color: white;
    height: 95px;
    font-weight: bold;
    width: 95px;
    display: table;
    margin: 10px auto;
  }
  .circle p {
    vertical-align: middle;
    display: table-cell;
  }

</style>
