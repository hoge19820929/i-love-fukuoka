<template>
  <div id="list" class="item-contents">
    <div v-for="(company, index) in companys" :key="company.id">
      <div class="list-contents">
        <div class="editCompany button" @click="editData(index, company.name)">編 集</div>
        <div class="button" @click="deleteData(index, company.name)">削 除</div>
        <div class="company-link" @click="detailData(index)">{{ company.name }}</div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'

export default {
  mounted() {
    this.init()
  },
  computed: {
    ...mapGetters({'companys' : 'companys', 'company' : 'company'})
  },
  methods: {
    async init() {
      await this.fetchCompanys()
      this.clearCompany()
    },
    async detailData(index) {
      this.fetchCompany({ index : index })
    },
    async editData(index, name) {
      const masterList = [
        'Yahoo JAPAN',
        'ペンシル',
        'ヌーラボ',
        'しくみデザイン',
        'GMOペパボ',
        'エニセンス',
        'キャッチアップ',
        'さくらインターネット',
        'クラウドエース',
        'ラック',
        'レベルファイブ',
        'クラスメソッド',
        'チームラボ',
        'ヤマップ',
        'オンサイト',
        'アジアクエスト'
      ]
      if (masterList.find(item => item === name)) {
        alert('マスターデータは変更できません')
        return
      }
      this.fetchCompany({ index : index })
      this.$router.push(`/edit`)
    },
    async deleteData(index, name) {
      const masterList = [
        'Yahoo JAPAN',
        'ペンシル',
        'ヌーラボ',
        'しくみデザイン',
        'GMOペパボ',
        'エニセンス',
        'キャッチアップ',
        'さくらインターネット',
        'クラウドエース',
        'ラック',
        'レベルファイブ',
        'クラスメソッド',
        'チームラボ',
        'ヤマップ',
        'オンサイト',
        'アジアクエスト'
      ]
      if (masterList.find(item => item === name)) {
        alert('マスターデータは削除できません')
        return
      }
      try {
        await this.deleteCompany({ index : index })
        this.$notify({
          type: 'success',
          title: '削除成功',
          position: 'bottom-right',
          duration: 1000
        })
      } catch (e) {
        this.$notify.error({
          title: '削除失敗',
          position: 'bottom-right',
          duration: 1000
        })
      }
      this.clearCompany()
    },
    ...mapActions(['fetchCompanys', 'fetchCompany', 'deleteCompany', 'clearCompany'])
  }
}
</script>

<style scoped>
#list {
  width: 35vw;
  min-height: 60vh;
}
.list-contents {
  margin: 15px 0;
  padding-bottom: 15px;
  border-collapse:separate;
  border-spacing: 15px 0;
  border-bottom: solid 1px #c0c0c0;
}
.list-contents div {
  display:table-cell;
  vertical-align: middle;
}
.editCompany {
  background-color: #6495ed;
}
.company-link {
  color: #6495ed;
}
.company-link:hover {
  color: #ff69b4;
}
</style>
