<template>
  <div>
     <!-- <vue-js :data="orgInfoList" multiple allow-batch whole-row @item-click="itemClick"></vue-js> -->
    <json-view 
    :data="orgInfoList" 
    rootKey="오스템임플란트(주)" 
    :maxDepth="1" 
    :styles="{ key: '#0977e6' }"/>
  </div>
</template>

<script>
  import axios from 'axios'
  import _ from 'lodash'
  import { JSONView } from "vue-json-component";
  import VueJs from 'vue-jstree';

  export default {
    components: {
        "vue-js": VueJs,
        "json-view": JSONView
    },
    data() {
      return {
        orgInfoList: {},
        data: []
      }
    },
    methods: {
        itemClick (node) {
          console.log(node.model.text + ' clicked !')
        }
      },
    created () {
      axios.get(`http://192.168.165.15:10080/api/orginfo/member/list/OSSTEM/0000`)
      .then((result) => {
        console.log(result);
        // this.data = result.data;
        this.orgInfoList = makeTree(result.data);
        
       }).catch((ex)=> {
        console.log("ERR!!!!!!! : ", ex)
        console.log(_.compact([0,1,true,2,'',3]));
      })
      
    },
    
  }

  /* eslint-disable */
  function makeTree(data) {
    var orgInfoList = data.orgInfoList;
    var orgMemberInfoList = data.orgMemberInfoList;

    var groupedByOrg = _.groupBy(orgInfoList, 'orgUpperCd');
    var groupedByIdMem = _.groupBy(orgMemberInfoList, 'orgCd');
    var catsByIdOrg = _.keyBy(orgInfoList, 'orgCd');
console.log(catsByIdOrg);
    _.each(groupedByIdMem, function(children, parentId) {
      if (catsByIdOrg[parentId] !== undefined){
        _.each(groupedByIdMem[parentId], function(val){
            catsByIdOrg[parentId][val.name] = val.jikgubName;
          })
      }
    });
    console.log(groupedByOrg);
    _.each(groupedByOrg, function(children, parentId) {
      if (catsByIdOrg[parentId] !== undefined){
        _.each(children, function(obj){
          // catsByIdOrg[parentId][obj.orgName] = obj; 
          catsByIdOrg[parentId][obj.orgName] = _.omit(obj, ['orgCd', 'orgUpperCd', 'orgName', 'orgLevel', 'seq']); 
          console.log(_.omit(obj, ['orgCd', 'orgUpperCd', 'orgName', 'orgLevel', 'seq']));
          if('경영기획1팀' ==obj.orgName ){
            console.log(catsByIdOrg[parentId][obj.orgName]);
            
            console.log(parentId + '  ' +obj.orgName);

          }
        if(parentId== "O000000841"){
          console.log(catsByIdOrg[parentId][obj.orgName]);
          
          console.log(obj.orgName);
          
        }
        });
      }
    });

    return _.omit(groupedByOrg['0'][0], ['orgName', 'orgCd', 'orgUpperCd', 'orgLevel', 'seq']);
  }

  /* eslint-disable */
  function makeTree2(data) {
    var orgInfoList = data.orgInfoList;
    var orgMemberInfoList = data.orgMemberInfoList;

    _.each(orgInfoList, function(val, idx){
      var filterOrg = _.filter(orgInfoList, {'orgUpperCd' :val.orgCd});
      var filterMem = _.filter(orgMemberInfoList, {'orgCd' :val.orgCd});

      _.each(filterMem, function(val2){
        val2.text = val2.name + '(' + val2.jikgubName + ')';
      });

      val.text = val.orgName;
      val.opened = false;
      val.children = _.concat(filterMem, filterOrg);
    });
    return [_.omit(_.find(orgInfoList, {'orgCd' : '0000'}), ['orgName', 'orgCd', 'orgUpperCd', 'orgLevel', 'seq'])];
  }

</script>

<style lang="scss" scoped>

</style>