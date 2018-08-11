<template>
  <div class="container p-0 bg-light" style="min-height: 100vh">

    <div class="row  p-2 justify-content-center align-items-center">
      <!--<a class="d-block btn text-white" :href="'/pages/jbdashboard/main'" role="button" style="background-color: #D78F30">-->
        <!--Start a New Ventures1-->
      <!--</a>-->
    </div>

    <template v-for="project in projects">
      <li>
        <div class="weui-btn-area">
          <a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0" :href="'/pages/jbdashboard/main?containerId='+project.containerId+'&projectName='+project.name">{{project.name}}</a>
        </div>
      </li>
    </template>

    <!--<div class="weui-btn-area">-->
      <!--<a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0" :href="'/pages/jbdashboard/main'">鹊山湖项目</a>-->
    <!--</div>-->

    <div class="weui-btn-area">
    <a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0">测试项目1</a>
    </div>

    <div class="weui-btn-area">
      <a class="pt-3 btn btn-lg text-white btn-block btn-primary mb-3 px-0">测试项目2</a>
    </div>
  </div>

</template>

<script>
import api from '@/api/index'

export default {
  data() {
    return {
      projects: []
    }
  },
  mounted() {
    this.listProjects()
  },
  methods: {
    listProjects() {
      // 获取项目列表接口
      api.get('/listProjects.do').then(response => {
        //console.log(JSON.stringify(response))
        //var result = response.result
        console.log(JSON.stringify(response))
        console.log(response[2])
        this.projects = response[2]
        //console.log(result['kie-containers']['kie-container'][0]['container-id'])
      }).catch(error => {
        console.log(error)
      })
    }
  }
}

</script>

<style>


</style>
