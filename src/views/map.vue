<template>
    <div class="main-map-w">
        <p>D3地图</p>
        <div class="main-w">
            <!-- <svg id="map-svg" :width="sWidth+'px'" :height="sHeight+'px'"></svg> -->
        </div>
    </div>
</template>
<script>
export default {
    name: "",
    data() {
        return {
            sWidth: "500",
            sHeight: "500"
        };
    },
    method: {},
    mounted() {

        const data = [
            {
                rect1: [[0, 0], [4, 4], [5, 5], [8, 8], [10, 10]]
            }
        ];
        var initWidth = 340;
        var initHeight = 500;

        const padding = { left: 40, top: 20, right: 20, bottom: 20 };

        const height = initWidth - padding.top - padding.bottom;
        const width = initHeight - padding.left - padding.right;

        let svg = d3
            .select(".main-w")
            .append("svg")
            .attr("width", width)
            .attr("height", height)
            .style("padding-left", padding.left)
            .style("padding-right", padding.right)
            .style("padding-top", padding.top)
            .style("padding-bottom", padding.bottom);

        //添加y轴坐标轴

        //y轴比例尺
        let nums = []; //把y轴方向的数据放在一个数组内，取最大最小值确定y轴比例
        data.forEach((e, i) => {
            for (var v in e) {
                nums = [...nums, ...e[v]];
            }
        });
        let ydata = nums.map(function(e, i) {
            console.log(e[1], i);
            return e[1];
        });
        let yScale = d3
            .scaleLinear()
            .domain([d3.min(ydata), d3.max(ydata)])
            .range([height, 0]);

        let _yScale = d3
            .scaleLinear()
            .domain([d3.min(ydata), d3.max(ydata)])
            .range([0, height]);

        //定义y轴
        let yAxis = d3.axisLeft(yScale);

        //添加y轴
        svg
            .append("g")
            .attr("class", "axis")
            .attr("transform", "translate(" + 0 + "," + 0 + ")")
            .call(yAxis);

        //添加x轴坐标轴

        //x轴比例尺
        let xData = nums.map(function(e, i) {
            return e[0];
        });
        let xScale = d3
            .scaleLinear()
            .domain([d3.min(xData), d3.max(xData)])
            .range([0, width]);

        //定义x轴
        let xAxis = d3.axisBottom(xScale);

        //添加x轴
        svg
            .append("g")
            .attr("class", "axis--x")
            .attr("transform", "translate(" + "0 ," + height + ")")
            .call(xAxis);

        // 定义横轴网格线
        function make_x_gridlines() {
            return d3.axisBottom(xScale).ticks(4); //设定坐标轴的分隔数
        }

        // 定义纵轴网格线
        function make_y_gridlines() {
            return d3.axisLeft(yScale).ticks(4);
        }

        // 添加横轴网格线、
        svg
            .append("g")
            .attr("class", "grid")
            .attr("transform", "translate(0," + height + ")")
            .call(
                make_x_gridlines()
                    .tickSize(-height) //设定坐标轴内外刻度的长度
                    .tickFormat("") //让所有的tick都没有标注
            );

        // 添加纵轴网格线
        svg
            .append("g")
            .attr("class", "grid")
            .call(
                make_y_gridlines()
                    .tickSize(-width)
                    .tickFormat("")
            );
    }
};
</script>
<style lang="less" scoped>
.main-map-w {
    #map-svg {
        display: block;
        border: solid 1px;
        margin: 0 auto;
    }
}
</style>


