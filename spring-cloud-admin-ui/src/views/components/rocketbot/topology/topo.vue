<template>
  <div class="micro-topo">
    <div class="micro-topo-container" ref="topo"></div>
  </div>
</template>
<script lang="js">
import * as d3 from 'd3';
/* eslint-disable */
/* tslint:disable */
const diagonal = d3.linkHorizontal().source(d => {
  d.sx = d.source.x;
  d.sy = d.source.y;
  if (d.source.x > d.target.x) {
    return [d.source.x, d.source.y];
  }
    d.sx = d.source.x + (d.source.name.length * 8 + 55);
  return [d.source.x + (d.source.name.length * 8 + 55), d.source.y];
}).target(d => {
  d.tx = d.target.x;
  d.ty = d.target.y;
  if (d.source.x < d.target.x) {
    return [d.target.x, d.target.y];
  }
  d.tx = d.target.x + (d.target.name.length * 8 + 55);
  return [d.target.x + (d.target.name.length * 8 + 55), d.target.y];
});
const diagonalvertical = d3.linkVertical().source(d => {
  d.sx = d.source.x;
  d.sy = d.source.y;
  if (d.source.x > d.target.x) {
    return [d.source.x, d.source.y];
  }
  d.sx = d.source.x + (d.source.name.length * 8 + 55);
  return [d.source.x + (d.source.name.length * 8 + 55), d.source.y];
}).target(d => {
  d.tx = d.target.x;
  d.ty = d.target.y;
  if (d.source.x < d.target.x) {
    return [d.target.x, d.target.y];
  }
  d.tx = d.target.x + (d.target.name.length * 8 + 55);
  return [d.target.x + (d.target.name.length * 8 + 55), d.target.y];
});

export default {
  props: {
    datas: {
      type: Object,
      default() {
        return {
          nodes: [],
          calls: [],
        };
      },
    },
  },
  data() {
    return {
      USER: require('./assets/USER.png'),
      UNKNOWN: require('./assets/UNKNOWN.png'),
      UNKNOWN_CLOUD: require('./assets/UNKNOWN_CLOUD.png'),
      USER1: require('./assets/USER1.png'),
      UNDEFINED: require('./assets/UNDEFINED.png'),
      KAFKACONSUMER: require('./assets/kafka.png'),
      KAFKA: require('./assets/kafka.png'),
      H2:require('./assets/DATABASE.png'),
      REDIS:require('./assets/REDIS.png'),
      TOMCAT: require('./assets/TOMCAT.png'),
      HTTPCLIENT: require('./assets/www(1).png'),
      DUBBO: require('./assets/DUBBO_PROVIDER.png'),
      MOTAN: require('./assets/DATABASE.png'),
      RESIN: require('./assets/RESIN.png'),
      FEIGN: require('./assets/www(1).png'),
      OKHTTP: require('./assets/www(1).png'),
      SPRINGRESTTEMPLATE: require('./assets/www(1).png'),
      SPRINGMVC: require('./assets/SPRING_BOOT.png'),
      STRUTS2: require('./assets/DATABASE.png'),
      NUTZMVC: require('./assets/SPRING_BOOT.png'),
      NUTZHTTP: require('./assets/www(1).png'),
      JETTYCLIENT:require('./assets/www(1).png'),
      JETTYSERVER: require('./assets/SPRING_BOOT.png'),
      SHARDINGJDBC: require('./assets/ShardingJDBC.png'),
      GRPC: require('./assets/GRPC.png'),
      ELASTICJOB: require('./assets/ElasticJob.png'),
      HTTPASYNCCLIENT: require('./assets/www(1).png'),
      DUBBO_PROVIDER: require('./assets/DUBBO_PROVIDER.png'),
      DUBBO_PROVIDER_GROUP: require('./assets/DUBBO_PROVIDER_GROUP.png'),
      ServiceComb: require('./assets/ORACLE_GROUP.png'),
      NG: require('./assets/ng.png'),
      NBASE: require('./assets/NBASE.png'),
      NBASE_T: require('./assets/NBASE_T.png'),
      NBASE_ARC: require('./assets/NBASE_ARC.png'),
      NBASE_ARC_GROUP: require('./assets/NBASE_ARC_GROUP.png'),
      MYSQL: require('./assets/MYSQL.png'),
      MYSQL_GROUP: require('./assets/MYSQL.png'),
      MSSQLSERVER: require('./assets/MSSQLSERVER.png'),
      MSSQLSERVER_GROUP: require('./assets/MSSQLSERVER_GROUP.png'),
      MONGODB: require('./assets/MONGODB.png'),
      MONGODB_GROUP: require('./assets/MONGODB_GROUP.png'),
      MEMCACHED: require('./assets/MEMCACHED.png'),
      MARIADB: require('./assets/MARIADB.png'),
      MARIADB_GROUP: require('./assets/MARIADB_GROUP.png'),
      JETTY: require('./assets/JETTY.png'),
      JBOSS: require('./assets/JBOSS.png'),
      FILTER: require('./assets/filter.png'),
      ETC: require('./assets/ETC.png'),
      DUBBO_PROVIDER: require('./assets/DUBBO_PROVIDER.png'),
      DUBBO_PROVIDER_GROUP: require('./assets/DUBBO_PROVIDER_GROUP.png'),
      CUBRID: require('./assets/CUBRID.png'),
      CUBRID_GROUP: require('./assets/CUBRID_GROUP.png'),
      CLIENT: require('./assets/CLIENT.png'),
      CASSANDRA: require('./assets/CASSANDRA.png'),
      BLOC: require('./assets/BLOC.png'),
      BACKEND: require('./assets/BACKEND.png'),
      ARCUS: require('./assets/ARCUS.png'),
      APACHE: require('./assets/APACHE.png'),
      ACTIVEMQ_CLIENT: require('./assets/ACTIVEMQ_CLIENT.png'),
      ACTIVEMQ_CLIENT_GROUP: require('./assets/ACTIVEMQ_CLIENT_GROUP.png'),
      width: 600,
      height: 600,
      force: '',
      svg: '',
      graph: '',
      link: '',
      node: '',
      zoom: '',
    };
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.resize);
    // this.$store.commit('skywalking/setCurrentNode', []);
  },
  mounted() {
    window.addEventListener('resize', this.resize);
    this.width = this.$refs.topo.offsetWidth;
    this.height = document.body.clientHeight - 98;
    this.svg = d3
      .select(this.$refs.topo)
      .append('svg')
      .attr('width', this.width)
      .attr('height', this.height);
  },
  watch: {
    'datas.nodes': 'draw',
  },
  methods: {
    draw() {
      this.svg.select('.graph').remove();
      this.force = d3
        .forceSimulation(this.datas.nodes)
        .force('collide', d3.forceCollide().radius(() => 90))
        .force('yPos', d3.forceY().strength(1))
        .force('xPos', d3.forceX().strength(1))
        .force('charge', d3.forceManyBody().strength(-500))
        .force( 'link', d3.forceLink(this.datas.calls).id(d => d.id))
        .force('center', d3.forceCenter(this.width / 2, this.height / 2))
        .on('tick', this.tick)
        .stop();
      this.graph = this.svg.append('g').attr('class', 'graph');
      this.svg.call(this.getZoomBehavior(this.graph));
      this.svg.on('click', (d, i) => {
        this.$emit('setCurrentApp', {name: '', id: ''});
      });
      this.defs = this.graph.append('defs');
      this.arrowMarker = this.defs
        .append('marker')
        .attr('id', 'arrow')
        .attr('markerUnits', 'strokeWidth')
        .attr('markerWidth', '12')
        .attr('markerHeight', '12')
        .attr('viewBox', '0 0 12 12')
        .attr('refX', '11')
        .attr('refY', '6')
        .attr('orient', 'auto');
      const arrow_path = 'M2,2 L10,6 L2,10 L6,6 L2,2';
      this.glink = this.graph.append('g').selectAll('.link');
      this.link = this.glink.data(this.datas.calls).enter().append('g');
      this.line = this.link.append('path').attr('class', 'link').attr("marker-end","url(#arrow)");
      this.linkText = this.link.append('g');
      this.linkText
        .append('rect')
        .attr('rx', 10)
        .attr('ry', 10)
        .attr('width', 20)
        .attr('height', 20)
        .attr('x', -10)
        .attr('y', -10)
        .attr('fill', '#2e303a');
      this.linkText
        .append('text')
        .attr('font-size', 10)
        .attr('class', 'linkText')
        .attr('text-anchor', 'middle')
        .attr('y', 3)
        .text(d => d.cpm);
      this.arrowMarker.append('path').attr('d', arrow_path).attr('fill', 'rgb(230, 170, 20)');
      this.gnode = this.graph.append('g').selectAll('.node');
      const that = this;
      this.node = this.gnode.data(this.datas.nodes)
        .enter()
        .append('g')
        .call(d3.drag()
            .on('start', this.dragstart)
            .on('drag', this.dragged)
            .on('end', this.dragended)
        )
        .on('click', function(d, i) {
          event.stopPropagation();
          that.node.attr('class', '');
          d3.select(this).attr('class', 'node-active');
          const copyD = JSON.parse(JSON.stringify(d));
          delete copyD.x;
          delete copyD.y;
          delete copyD.vx;
          delete copyD.vy;
          delete copyD.fx;
          delete copyD.fy;
          delete copyD.index;
          that.$emit('setCurrentApp', copyD);
        });
      this.node
        .append('rect')
        .attr('class', d => (d.type === 'out' ? 'out' : 'node'))
        .attr('rx', 17)
        .attr('ry', 17)
        .attr('width', d => d.name.length * 8 + 55)
        .attr('height', 34);
      this.node
        .append('image')
        .attr('width', 20)
        .attr('height', 20)
        .attr('style', 'cursor: move;')
        .attr('x', 10)
        .attr('y', 7)
        .attr('xlink:href',d => this[d.type.toUpperCase().replace('-','')]);
      this.node
        .append('text')
        .attr('font-size', 10)
        .attr('text-anchor', 'middle')
        .attr('x', d => d.name.length * 8 + 40)
        .attr('y', 21)
        .attr('font-weight', 600)
        .text(d => {
          if(!d.sla || d.sla === 100) return;
          const rate = 100 - d.sla;
          return rate + '%' })
        .attr('fill', 'rgb(240, 84, 20)');
      this.node
        .append('text')
        .attr('class', 'node-name')
        .attr('x', 38)
        .attr('font-size', 13)
        .attr('y', 21)
        .text(d => d.name)
        .attr('fill', '#fafafa');
      d3.timeout(() => {
        for (
          let i = 0,
            n = Math.ceil(
              Math.log(this.force.alphaMin()) /
                Math.log(1 - this.force.alphaDecay())
            );
          i < n;
          i += 1
        ) {
          this.force.tick();
          this.tick();
        }
      });
    },
    resize() {
      this.svg.attr('width', this.$refs.topo.offsetWidth);
      this.svg.attr('height', document.body.clientHeight - 98);
    },
    tick() {
      this.line
        .attr('stroke-width', 1)
        .attr(
          'd',
          d => {
            if(Math.abs(d.sx-d.tx) > Math.abs(d.sy-d.ty)) return diagonal(d);
            if(Math.abs(d.sx-d.tx) < Math.abs(d.sy-d.ty)) return diagonalvertical(d);
            return diagonal(d);
          }
        );
      this.linkText.attr('transform',d => {
        let tagx = 1
        // if(d.sx < d.tx) tagx = -tagx;
        return `translate(${d.sx - (d.sx-d.tx)/2}, ${d.sy - (d.sy-d.ty)/2})`
      });
      this.node.attr('transform', d => `translate(${d.x},${d.y - 20})`);
    },
    getZoomBehavior(g) {
      return d3
        .zoom()
        .scaleExtent([0.3, 10])
        .on('zoom', () => {
          g.attr(
            'transform',
            `translate(${d3.event.transform.x},${d3.event.transform.y})scale(${
              d3.event.transform.k
            })`
          );
        });
    },
    dragstart(d) {
      // console.log(this.node)
      this.node._groups[0].forEach(d => {
        d.__data__.fx = d.__data__.x;
        d.__data__.fy = d.__data__.y;
      });
      if (!d3.event.active) {
        this.force.alphaTarget(0.01).restart();
      }
      d3.event.sourceEvent.stopPropagation();
    },
    dragged(d) {
        d.fx = d3.event.x;
        d.fy = d3.event.y;
    },
    dragended() {
      if (!d3.event.active) {
        this.force.alphaTarget(0);
      }
    },
  },
};
</script>
<style lang="scss">
.micro-topo {
  .micro-topo-container {
    text-align: center;
    border-radius: 2px;
    overflow: hidden;
  }
  .node-name {
    cursor: move;
  }
  .link {
    stroke-linecap: round;
    fill:rgba(255, 255, 255, 0);
    stroke: rgba(255, 199, 31, 0.5);
    stroke-dasharray: 30 3;
    animation: dash 6s linear infinite;
  }
@keyframes dash {
    from {
      stroke-dashoffset: 330;
    }
    to {
      stroke-dashoffset: 0;
    }
  }
  .link2 {
    stroke: rgb(80, 80, 80);
    stroke-dasharray: 200 20;
    stroke-dashoffset: 0;
    animation: dash 1.5s linear infinite;
  }
  @keyframes dash {
    from {
      stroke-dashoffset: 220;
    }
    to {
      stroke-dashoffset: 0;
    }
  }
  .linkText {
    font-family: SimSun;
    fill: #eee;
  }
  .user {
    cursor: move;
    fill: #3890ff;
  }
  .user-content {
    cursor: move;
    fill: #217ef2;
  }
  .out {
    cursor: move;
    fill: #a330ec;
  }
  .out-title {
    cursor: move;
    fill: #9026d4;
  }
  .node {
    cursor: move;
    fill: #3e414b;
  }
  .node-active {
    .node {
      stroke-width: 2;
      stroke: rgba(255, 255, 255, 0.5);
    }
  }
  .node-title {
    cursor: move;
    fill: #4c4c4e;
  }
}
</style>
