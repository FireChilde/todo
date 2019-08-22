<template>
  <div>
    <json-view 
    :data="orgInfoList"
    rootKey="view"
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
        this.orgInfoList = { '오스템' : makeCatTree(result.data.orgInfoList)};
        this.orgMemberInfoList = { orgMemberInfoList : result.data.orgMemberInfoList};

       }).catch((ex)=> {
        console.log("ERR!!!!!!! : ", ex)
        console.log(_.compact([0,1,true,2,'',3]));
      })
      
    },
    
  }

  function makeCatTree(result) {
    console.log("---");
    var groupedByParents = _.groupBy(result, 'orgUpperCd');
    var catsById = _.keyBy(result, 'orgCd');
    console.log(groupedByParents);
    _.each(_.omit(groupedByParents, '0000'), function(children, parentId) {
      if (catsById[parentId] != null){
        catsById[parentId].children = children; 
      }
    });
    _.each(catsById, function(cat) {
      // isParent will be true when there are subcategories (this is not really a good name, btw.)
      cat.isParent = !_.isEmpty(cat.children); 
      // _.compact below is just for removing null posts
      cat.children = _.compact(_.union(cat.children, cat.posts));
      // optionally, you can also delete cat.posts here.
    });
    console.log(groupedByParents['0000']);
    console.log("---");
    return groupedByParents['0000'];
  }
</script>

<style lang="scss" scoped>

</style>