<template>
  <div>
    <json-view 
    :data="orgInfoList"
    rootKey="오스템임플란트(주)"
    :maxDepth="1"
    :styles="{ key: '#0977e6' }"
     />
  </div>
</template>

<script>
  import axios from 'axios'
  import _ from 'lodash'
  import { JSONView } from "vue-json-component";

  export default {
    components: {
        "json-view": JSONView 
    },
    data() {
      return {
        orgInfoList: {},
        data:{}
      }
    },
    created () {
      axios.get(`http://192.168.165.15:10080/api/orginfo/member/list/OSSTEM/0000`)
      .then((result) => {
        console.log(result);
        this.data = result.data;
        this.orgInfoList = makeTree(result.data);
        
        console.log(this.orgInfoList);
       }).catch((ex)=> {
        console.log("ERR!!!!!!! : ", ex)
        console.log(_.compact([0,1,true,2,'',3]));
      })
      
    },
    
  }

  function makeTree(data) {
    var orgInfoList = data.orgInfoList;
    var orgMemberInfoList = data.orgMemberInfoList;

    var groupedByParents = _.groupBy(orgInfoList, 'orgUpperCd');
    var groupedByIdMem = _.groupBy(orgMemberInfoList, 'orgCd');
    var catsByIdOrg = _.keyBy(orgInfoList, 'orgCd');

    _.each(groupedByIdMem, function(children, parentId) {
      if (catsByIdOrg[parentId] != null){
        _.each(groupedByIdMem[parentId],function(val){
            catsByIdOrg[parentId][val.name] = val.jikgubName;
          })
      }
    });
    _.each(_.omit(groupedByParents, '0'), function(children, parentId) {
      if (catsByIdOrg[parentId] != null){
        _.each(children,function(obj){
          catsByIdOrg[parentId][obj.orgName] =  _.omit(obj, ['orgCd','orgUpperCd','orgName','orgLevel','seq']); 
        });

      }
    });

    return _.omit(groupedByParents['0'][0], ['orgName','orgCd','orgUpperCd','orgLevel','seq']);
  }

  function makeTree2(data) {
    var orgInfoList = data.orgInfoList;
    var orgMemberInfoList = data.orgMemberInfoList;

    var catsByIdOrg = _.keyBy(orgInfoList, 'orgCd');

    _.each(catsByIdOrg, function(val, idx){
      _.each(_.filter(orgInfoList, {'orgUpperCd' :idx}), function(val2){
        val[val2.orgName] = val2;
      });
      _.each(_.filter(orgMemberInfoList, {'orgCd' :idx}), function(val2){
        val[val2.name] = val2.jikgubName;
      });
    });

    return catsByIdOrg['0000'];
  }
</script>

<style lang="scss" scoped>

</style>