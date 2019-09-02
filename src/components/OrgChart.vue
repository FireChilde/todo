<template>
  <div>
    <json-view 
    :data="orgInfoList"
    rootKey="오스템임플란트"
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
        orgMemberInfoList: {
          first: "level",
          works: true,
          object: {
            working: "properly"
          },
          number: 100,
          missing: null,
          list: [
            "fun",
            {
              test: {
                passed: true
              }
            }
          ]
         
        },
        data:{first : 'jk'},
        test:{
          first: "level",
          works: true,
          object: {
            working: "properly"
          },
          number: 100,
          missing: null,
          list: [
            "fun",
            {
              test: {
                passed: true
              }
            }
          ]
         
        }
    }
    },
    created () {
      axios.get(`http://192.168.165.15:10080/api/orginfo/member/list/OSSTEM/0000`)
      .then((result) => {
        this.data = result.data;
        this.orgInfoList = makeTree(result.data);
        this.orgMemberInfoList = { orgMemberInfoList : result.data.orgMemberInfoList};
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
    var catsByIdOrg = _.keyBy(orgInfoList, 'orgCd');
    var catsByIdMem = _.keyBy(orgMemberInfoList, 'orgCd');

    _.each(_.omit(groupedByParents, '0000'), function(children, parentId) {
      if (catsByIdOrg[parentId] != null){
        _.each(children,function(obj){
          catsByIdOrg[parentId][obj.orgName] =  obj; 
        });

      }
    });
    _.each(catsByIdOrg, function(cat) {
      var catFind = _.find(catsByIdMem, {orgCd:cat.orgCd});
      
      if(catFind != undefined){
        cat[catFind.name] = catFind.jikgubName;
      }

    });
    return _.omit(groupedByParents['0000'], ['orgCd','orgUpperCd','orgLevel','seq']);
  }
</script>

<style lang="scss" scoped>

</style>