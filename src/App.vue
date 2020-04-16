<template>
  <div id="app">
    <h1 class="logo">全村的希望-爆点</h1>
    <el-form ref="form" label-width="80px">
      <el-form-item label="单双">
        <el-radio-group v-model="shuangdan" label="单双排">
          <el-radio-button label="1">双排</el-radio-button>
          <el-radio-button label="2">单排</el-radio-button>
        </el-radio-group>
      </el-form-item>
      <el-form-item label="模式">
        <el-radio-group v-model="moshi" label="模式" @change="radioChange">
          <el-radio-button label="xingdong">星动</el-radio-button>
          <el-radio-button label="xuanyue">玄月</el-radio-button>
          <el-radio-button label="tanzhu">弹珠</el-radio-button>
        </el-radio-group>
      </el-form-item>
    </el-form>
    <div class="search-box">
      <el-input placeholder="请输入歌名字母" suffix-icon="el-icon-search" v-model="input" @input="inputSearch"></el-input>
    </div>
    <el-table :data="tableData" header-align="center" highlight-current-row>
      <el-table-column align="center" prop="letter" label="字母" width="40"></el-table-column>
      <el-table-column align="center" prop="level" label="等级" width="40"></el-table-column>
      <el-table-column align="left" prop="name" label="歌名"></el-table-column>
      <el-table-column v-if="shuangdan === '1'" align="center" prop="shuang" label="双爆" class="bold"></el-table-column>
      <el-table-column v-else align="center" prop="dan" label="单爆" class="bold"></el-table-column>
      <el-table-column align="center" prop="lei" label="类型" width="50"></el-table-column>
    </el-table>
  </div>
</template>

<script>
import Fuse from 'fuse.js'
import musicData from '@/data/data.json'

export default {
  name: 'App',
  data: () => ({
    shuangdan: '1',
    moshi: 'xingdong',
    input: null,
    tableData: null,
    fuseSearch: null
  }),
  created () {
    this.tableData = musicData[this.moshi]
    this.fuseSearch = new Fuse(musicData[this.moshi], {
      keys: ['letter', 'level'],
      threshold: 0.3
    })
  },
  methods: {
    radioChange (item) {
      this.tableData = musicData[this.moshi]
      this.fuseSearch = new Fuse(musicData[this.moshi], {
        keys: ['letter', 'level'],
        threshold: 0.3
      })
      this.input = null
    },
    inputSearch (query) {
      let result = []
      if (query !== '') {
        result = this.fuseSearch.search(query)
      } else {
        this.tableData = musicData[this.moshi]
      }
      this.tableData = result
    }
  }
}
</script>
