<template>
	<div class="chart">
		<h1>Info:</h1>
		<svg width="960" height="500"></svg>
		<p>{{ info }}</p>
	</div>

</template>
<script>

var d3 = require('d3');
import axios from 'axios';

export default {
	name: 'Chart',
	data() {
		return {
			info: null,
		}
	},
	mounted() {
		axios
			.get('https://api.gdax.com/products/BTC-USD/candles')
			.then(response => {
				this.info = response.data;
				var svg = d3.select('svg');
				var margin = {top: 20, right: 20, bottom: 30, left: 50};
				var width = svg.attr("width") - margin.left - margin.right;
				var height = svg.attr("height") - margin.top - margin.bottom;
				var g = svg.append("g").attr("transform", "translate(" + margin.left + "," + margin.right + ")");
		
				var x = d3.scaleTime().rangeRound([0, width]);
				var y = d3.scaleLinear().rangeRound([height, 0]);
		
				var line = d3.line()
					.x(function(d) { return x(d[0]); })
					.y(function(d) { return y(d[4]); });
		
				//d3.data(this.info);
				//d3.data(this.info, function(d) { return d; }, function(error, data) {
					//if(error) throw error;
					
					x.domain(d3.extent(this.info, function(d) { return d[0]; }));
					y.domain(d3.extent(this.info, function(d) { return d[4]; }));
		
					g.append("g")
						.attr("transform", "translate(0, " + height + ")")
						.call(d3.axisBottom(x))
						.select(".domain")
						.remove();
		
					g.append("g")
						.call(d3.axisLeft(y))
						.append("text")
						.attr("fill", "#000")
						.attr("transform", "rotate(-90)")
						.attr("y", 6)
						.attr("dy", "0.71em")
						.attr("text-anchor", "end")
						.attr("Price ($)");
		
					g.append("g")
						.datum("data")
						.attr("fill", "none")
						.attr("stroke", "steelblue")
						.attr("stroke-linejoin", "round")
						.attr("stroke-linecap", "round")
						.attr("stroke-width", 1.5)
						.attr("d", line);
				//});
		
			})
			.catch(error => {
				console.log(error);
			});
		
//		var svg = d3.select('svg');
//		var margin = {top: 20, right: 20, bottom: 30, left: 50};
//		var width = svg.attr("width") - margin.left - margin.right;
//		var height = svg.attr("height") - margin.top - margin.bottom;
//		var g = svg.append("g").attr("transform", "transform(" + margin.left + "," + margin.right + ")");
//
//		var x = d3.scaleTime().rangeRound([0, width]);
//		var y = d3.scaleLinear().rangeRound([height, 0]);
//
//		var line = d3.line()
//			.x(function(d) { return x(d[0]); })
//			.y(function(d) { return y(d[4]); });
//
//		//d3.data(this.info);
//		//d3.data(this.info, function(d) { return d; }, function(error, data) {
//			//if(error) throw error;
//			
//			x.domain(d3.extent(this.info, function(d) { return d[0]; }));
//			y.domain(d3.extent(this.info, function(d) { return d[4]; }));
//
//			g.append("g")
//				.attr("transform", "translate(0, " + height + ")")
//				.call(d3.axisBottom(x))
//				.select(".domain")
//				.remove();
//
//			g.append("g")
//				.call(d3.axisLeft(y))
//				.append("text")
//				.attr("fill", "#000")
//				.attr("transform", "rotate(-90)")
//				.attr("y", 6)
//				.attr("dy", "0.71em")
//				.attr("text-anchor", "end")
//				.attr("Price ($)");
//
//			g.append("g")
//				.datum("data")
//				.attr("fill", "none")
//				.attr("stroke", "steelblue")
//				.attr("stroke-linejoin", "round")
//				.attr("stroke-linecap", "round")
//				.attr("stroke-width", 1.5)
//				.attr("d", line);
//		//});
	}
}

</script>
