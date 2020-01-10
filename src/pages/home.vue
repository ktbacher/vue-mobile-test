<template>
  <f7-page>
    <f7-navbar id="nav" sliding large>
      <f7-link icon-f7="bars" open-panel="left"></f7-link>
      <f7-nav-center sliding>AttentivU</f7-nav-center>
      <f7-nav-right>
        <f7-link icon="icon-bars" panel-open="right"></f7-link>
      </f7-nav-right>
    </f7-navbar>
    <f7-block-title id="title">{{ view }}</f7-block-title>
    <f7-block id="contatiner" inner>
      <!-- Some text -->
      <!-- <Test/> -->
      <div class='gauges' v-if="view=='Current'">
        <Gauge class='gauge' :chart_id='chart1_id' :metric='chart1_metric'/>
        <Gauge class='gauge' :chart_id='chart2_id' :metric='chart2_metric'/>
        <Gauge class='gauge' :chart_id='chart3_id' :metric='chart3_metric'/>
      </div>
      <div class='lines' v-if="view=='Hours'">
        <LineGraph class='line-chart' :chart_id='chart4_id'/>
      </div>
    </f7-block>
      
  </f7-page>
</template>
<script>
import Test from '../components/Test'
import Gauge from '../components/Gauge'
import LineGraph from '../components/LineGraph'

export default {
  name: 'Home',
  data() {
    return {
      view: 'Hours',
      chart1_id: 'chart1',
      chart1_metric: 'Focus',
      chart2_id: 'chart2',
      chart2_metric: 'Load',
      chart3_id: 'chart3',
      chart3_metric: 'Fatigue',
      chart4_id: 'chart4'
    };
  },
  components: {
    Gauge,
    Test,
    LineGraph
  },
};
</script>

<style scoped>
#nav {
  background-color: rgba(231, 232, 245, 0.89);
  color: black;
}
.gauges {
  width: 70%
}
#container {
  display: flex;
  margin-top:2rem;
  /* justify-content: center; */
  flex-direction: column;
  /* align-items: center; */
  justify-content: flex-end;
  /* align-content: center; */


}
.gauges {
  width: 100%;
  display: flex;
  /* margin-top:2rem; */
  justify-content: center;
  flex-direction: column;
  /* align-items: center; */
  /* justify-content: flex-end; */

}

.lines {
  width: 100%;
  display: flex;
  /* margin-top:2rem; */
  justify-content: center;
  /* flex-direction: column; */
  /* align-items: center; */
  /* justify-content: flex-end; */

}

.gauge {
  width: 80%;
  /* margin: 2rem; */
  margin-bottom: 2rem;
}
.line-chart {
  width: 80%;
  /* margin: 2rem; */
  margin-bottom: 2rem;
}

#title {
  font-size: 12pt;
  margin-top: 2rem;
}
</style>
