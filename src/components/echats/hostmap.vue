<template>
<div>
<div id="hostmap" :style="style"></div>
  <div class="mapDiv">
    <a>总节点：<span>12</span> 个</a>
    <a>告警节点：<span style="color:red">2</span> 个</a>
    <a>正常节点：<span>10</span> 个</a>
    <a>最高温度：<span>80.5</span> 度</a>
    <a>报警阈值：<span>40</span> 度</a>
    </div>
  <!-- <div class="mapDivCenter">
    <a>正常设备占比：<span>88%</span></a>
    </div>-->
  <div class="mapDiv1">
    <a style="font-weight:bolder;margin-bottom:5px">图例说明</a>
    <a style="line-height:24px;color:#E74C3C;font-weight:bolder;" class="img1"><span style="color:#fff;font-weight:bolder;margin-left:50px">报警节点</span></a>
    <a style="line-height:24px;color:#89E73C;font-weight:bolder;" class="img2"><span style="color:#fff;font-weight:bolder;margin-left:50px">正常节点</span></a>
    </div>
</div>
</template>
<script> 
export default{
    data(){
      return{
        style:{
          width:'100%',
          height:'100%',
          position:'absolute',   
        }
       }
     
    },
  mounted(){
// 全局钩子
 var vueThis=this;
 var hostmap=document.getElementById('hostmap');
 hostmap.style.width=window.innerWidth+'px';
 hostmap.style.height=((window.innerHeight)*0.9)+'px';
var myChart = echarts.init(hostmap);
var geoCoordMap ={
 '38.5':[121.411147,31.178764],
 '39.5':[121.409992,31.178243],
 '37.5':[121.413475,31.184491],
 '36.5':[121.414904,31.182674],
 '35.5':[121.410404,31.16982],
 '34.5':[121.397616,31.170258],
 '80.5':[121.392763,31.170833],
 '81.5':[	121.411539,31.170511],
 '37.6':[121.404426,31.170689],	
 '38.6':[121.405324,31.172052],
 '39.6':[121.409445,31.177932],
 '35.6':[121.408121,31.17802],
 '79.5':[121.404895,31.177335],

};
var Data =[
    [{name:'创新大楼'},{name:'38.5',value:38.5,color:'#03FED2'}],
    [{name:'创新大楼'},{name:'39.5',value:39.5,color:'#03FED2'}],
    [{name:'创新大楼'},{name:'37.5',value:37.5,color:'#03FED2'}],
    [{name:'创新大楼'},{name:'36.5',value:36.5,color:'#03FED2'}],
    [{name:'创新大楼'},{name:'35.5',value:35.5,color:'#03FED2'}],
    [{name:'创新大楼'},{name:'34.5',value:34.5,color:'#03FED2'}],
    [{name:'创新大楼'},{name:'80.5',value:80.5,color:'red'}],
    [{name:'创新大楼'},{name:'81.5',value:81.5,color:'red'}],
    [{name:'创新大楼'},{name:'79.5',value:79.5,color:'red'}],
    [{name:'创新大楼'},{name:'38.6',value:38.6,color:'#03FED2'}],
    [{name:'创新大楼'},{name:'37.6',value:37.6,color:'#03FED2'}],
    [{name:'创新大楼'},{name:'39.6',value:39.6,color:'#03FED2'}],
    [{name:'创新大楼'},{name:'35.6',value:35.6,color:'#03FED2'}],

   



]
var convertData = function (data) {
    var res = [];
    for (var i = 0; i < data.length; i++) {
        var dataItem = data[i];
        var fromCoord = geoCoordMap[dataItem[1].name];
        var toCoord = geoCoordMap[dataItem[0].name];
        if (fromCoord && toCoord) {
            res.push({
                fromName: dataItem[0].name,
                toName: dataItem[1].name,
                coords: [fromCoord, toCoord]
            });
        }
    }
    return res;
};


var series = [];
[['创新大楼', Data]].forEach(function (item,i) {
    var color='';
    item[1].map(function (dataItem) {
      // console.log(dataItem[1].color);
    })
    series.push(
        {
            name:'详细信息',
            type: 'effectScatter',
            coordinateSystem: 'bmap',
            zlevel: 2,
            rippleEffect: {
                brushType: 'stroke'
            },
            label: {
                normal: {
                    show: true,
                    position: 'center',
                    formatter: '{b}'
                }
            },
            symbolSize: function (val) {
                return val[2] / 5;
            },
             data: item[1].map(function (dataItem) {
                return {
                    name: dataItem[1].name,
                    value: geoCoordMap[dataItem[1].name].concat([dataItem[1].value]),
                     itemStyle: {
                normal:{
                color:dataItem[1].color
                }
            },
                };
            })
        });
});

var option = {
    tooltip : {
        trigger: 'item'
    },
    bmap: {
      
        roam: true,
        silent:true,
        mapStyle:{
           styleJson: [
                {
                  'featureType': 'water',
                  'elementType': 'all',
                  'stylers': {
                    'color': '#031628'
                  }
                },
                {
                  'featureType': 'land',
                  'elementType': 'geometry',
                  'stylers': {
                    'color': '#000102'
                  }
                },
                {
                  'featureType': 'highway',
                  'elementType': 'all',
                  'stylers': {
                    'visibility': 'off'
                  }
                },
                {
                  'featureType': 'arterial',
                  'elementType': 'geometry.fill',
                  'stylers': {
                    'color': '#000000'
                  }
                },
                {
                  'featureType': 'arterial',
                  'elementType': 'geometry.stroke',
                  'stylers': {
                    'color': '#0b3d51'
                  }
                },
                {
                  'featureType': 'local',
                  'elementType': 'geometry',
                  'stylers': {
                    'color': '#000000'
                  }
                },
                {
                  'featureType': 'railway',
                  'elementType': 'geometry.fill',
                  'stylers': {
                    'color': '#000000'
                  }
                },
                {
                  'featureType': 'railway',
                  'elementType': 'geometry.stroke',
                  'stylers': {
                    'color': '#08304b'
                  }
                },
                {
                  'featureType': 'subway',
                  'elementType': 'geometry',
                  'stylers': {
                    'lightness': -70
                  }
                },
                {
                  'featureType': 'building',
                  'elementType': 'geometry.fill',
                  'stylers': {
                    'color': '#000000'
                  }
                },
                {
                  'featureType': 'all',
                  'elementType': 'labels.text.fill',
                  'stylers': {
                    'color': '#857f7f'
                  }
                },
                {
                  'featureType': 'all',
                  'elementType': 'labels.text.stroke',
                  'stylers': {
                    'color': '#000000'
                  }
                },
                {
                  'featureType': 'building',
                  'elementType': 'geometry',
                  'stylers': {
                    'color': '#022338'
                  }
                },
                {
                  'featureType': 'green',
                  'elementType': 'geometry',
                  'stylers': {
                    'color': '#062032'
                  }
                },
                {
                  'featureType': 'boundary',
                  'elementType': 'all',
                  'stylers': {
                    'color': '#465b6c'
                  }
                },
                {
                  'featureType': 'manmade',
                  'elementType': 'all',
                  'stylers': {
                    'color': '#022338'
                  }
                },
                {
                  'featureType': 'label',
                  'elementType': 'all',
                  'stylers': {
                    'visibility': 'off'
                  }
                },
                {
                    "featureType": "poi",
                    "elementType": "all",
                    "stylers": {
                              "color": "#03FED2",
                              "visibility": "off"
                    }
          }

              ]
        }
    },
    series: series
};
 
myChart.setOption(option);
var map = myChart.getModel().getComponent('bmap').getBMap();
    map.disableDoubleClickZoom();
    map.centerAndZoom(new BMap.Point(121.39908,31.177505), 17);  // 初始化地图
window.onresize = myChart.resize;//图表自适应


  }
  
  }
</script>
<style scoped>
#hostmap {width: 100%;overflow: hidden;margin:0;font-family:"微软雅黑";}
.mapDiv{width:340px;height:200px;position:absolute;top:100px;display: flex;justify-content: center;align-items:flex-start;flex-direction:column;
padding-left:40px;background: rgba(0, 0, 0, .4);box-shadow: 4px 5px 10px rgba(159, 158, 156, .6)}
.mapDiv a{height:36px;line-height: 36px;font-size: 18px;color:#fff;font-weight:bolder;}
.mapDiv a span{color:#fffc11;font-size: 24px;}


.mapDivCenter{width:340px;height:80px;position:absolute;top:100px;left:400px;display: flex;justify-content: center;align-items:flex-start;flex-direction:column;
padding-left:100px;background: rgba(0, 0, 0, .4);box-shadow: 4px 5px 10px rgba(159, 158, 156, .6)}
.mapDivCenter a{height:36px;line-height: 36px;font-size: 18px;color:#fff;font-weight:bolder;}
.mapDivCenter a span{color:#fffc11;font-size: 24px;}

.mapDiv1{width:200px;height:150px;position:absolute;display: flex;justify-content: center;align-items:flex-start;flex-direction:column;
padding-left:40px;bottom: 50px;right:50px;background: rgba(0, 0, 0, .5);box-shadow: 4px 5px 10px rgba(159, 158, 156, .6)}
.mapDiv1 a{height:36px;line-height: 36px;font-size: 18px;color:#fff}
.img1{background:url(../../assets/1.png) no-repeat;background-size:32px 30px}
.img2{background:url(../../assets/2.png) no-repeat;background-size:32px 30px}
.img3{background:url(../../assets/3.png) no-repeat;background-size:32px 30px}
.img4{background:url(../../assets/4.png) no-repeat;background-size:32px 30px}
</style>
