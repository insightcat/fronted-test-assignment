<template>
	<vue-draggable-resizable class="chartContainer"
							 :style="{left:current_x + 'px', top:current_y + 'px', width:current_w + 'px', height:current_h + 'px'}"
							 :handles="['br']"
							 :grid=[grid_step_x,grid_step_y]
							 @dragging="onDrag"
							 @resizing="onResize">
		<!-- title bar -->
		<div class="drag-handle" :style="{width:width + 'px'}">
			<div class="grid-title">{{name}}</div>
		</div>

		<!-- scale left -->
		<div class="chart-tex-y-left" :style="{width:current_w + 'px', height:current_h-20 + 'px'}">
			<div v-for="(y_scale_item) in y_scale">{{y_scale_item}}</div>
		</div>

		<!-- scale bottom -->
		<div class="chart-tex-x-bottom">
			<div v-for="(x_scale_item) in x_scale">{{x_scale_item}}</div>
		</div>

		<!-- grid and graphs -->
		<canvas class="image_canvas" ref="image_canvas" :width="current_w" :height="current_h"
				:style="{width:current_w + 'px', height:current_h + 'px'}">
		</canvas>

		<!-- overlay hints -->
		<canvas class="image_canvas_overlay" ref="image_canvas_overlay" :width="current_w" :height="current_h"
				v-bind:style="{width:current_w + 'px', height:current_h + 'px'}">
		</canvas>

		<!-- legends (small color bar and title (one or multiple legends) -->
		<div class="legends_container">
			<div class="one_legend" v-for="(obj, ind) in legends">
				<div class="legend-color-bar"></div>
				<div class="legend-text">{{ obj }}</div>
			</div>
		</div>
	</vue-draggable-resizable>
</template>

<script>
	export default {
		name: "TestTask",
		//Incoming properties
		props: {
			name: {
				type: String,
				default: "Test"
			},
			x: {
				type: Number,
				default: 0
			},
			y: {
				type: Number,
				default: 0
			},
			width: {
				type: Number,
				default: 482
			},
			height: {
				type: Number,
				default: 360
			},
			//default range 5 minutes, byt can be other
			StartUTC: {
				type: Number,
				default: 1596128655
			},
			EndUTC: {
				type: Number,
				default: 1596128955
			}
		},
		data()
		{
			return {
				current_x: 0,
				current_y: 0,
				current_w: 0,
				current_h: 0,

				grid_step_x: 115,
				grid_step_y: 60,
				x_scale: [1, 2, 3, 4],
				y_scale: [1, 2, 3, 4],
				legends: ["legend_1"],
				initialData: [
					{
						"metric":
							{
								"__name__": "cpu_usage_user",
								"cpu": "cpu-total",
								"host": "agent-test-1"
							},
						"values": [[1596128655, "0.325488232324025"], [1596128665, "0.32555095164044"], [1596128675, "0.25037562613318"], [1596128685, "0.300651704889"], [1596128695, "0.2756266798595"], [1596128705, "0.125150200286155"], [1596128715, "0.12515020030486002"], [1596128725, "0.17532577714181502"], [1596128735, "0.15026319578573"], [1596128745, "0.25047611524512"], [1596128755, "0.4007766034975"], [1596128765, "0.4510150380845"], [1596128775, "0.425977481762"], [1596128785, "0.375827030452"], [1596128795, "0.300777080739805"], [1596128805, "0.275475838990305"], [1596128815, "0.27553862122999995"], [1596128825, "0.350764185004755"], [1596128835, "0.325651302584055"], [1596128845, "0.2255136840193"], [1596128855, "0.22556390979234497"], [1596128865, "0.300551077209105"], [1596128875, "0.37573904713376"], [1596128885, "0.4507138717675"], [1596128895, "0.42542542542967"], [1596128905, "0.42583937322867"], [1596128915, "0.40081434820423"], [1596128925, "0.30056372159902"], [1596128935, "0.30056372159902"], [1596128945, "0.30056372159902"], [1596128955, "0.30056372159902"]]
					},
					{
						"metric":
							{
								"__name__": "cpu_usage_iowait",
								"cpu": "cpu-total",
								"host": "agent-test-1"
							},
						"values": [[1596128655, "0"], [1596128665, "0"], [1596128675, "0"], [1596128685, "0.025075225680575"], [1596128695, "0.025075225680575"], [1596128705, "0"], [1596128715, "0"], [1596128725, "0"], [1596128735, "0"], [1596128745, "0.025062656636125"], [1596128755, "0.025062656636125"], [1596128765, "0"], [1596128775, "0.02503755633654"], [1596128785, "0.02503755633654"], [1596128795, "0"], [1596128805, "0.02503755633654"], [1596128815, "0.02503755633654"], [1596128825, "0"], [1596128835, "0"], [1596128845, "0"], [1596128855, "0.025062656643085"], [1596128865, "0.025062656643085"], [1596128875, "0"], [1596128885, "0"], [1596128895, "0.025025025022735"], [1596128905, "0.025025025022735"], [1596128915, "0"], [1596128925, "0.025062656638405"], [1596128935, "0.025062656638405"], [1596128945, "0.025062656638405"], [1596128955, "0.025062656638405"]]
					},
					{
						"metric":
							{
								"__name__": "cpu_usage_system",
								"cpu": "cpu-total",
								"host": "agent-test-1"
							},
						"values": [[1596128655, "0.37556334501099997"], [1596128665, "0.22536309472726002"], [1596128675, "0.150200300490595"], [1596128685, "0.250551454174335"], [1596128695, "0.27557647920399997"], [1596128705, "0.15017522531588"], [1596128715, "0.07510015027288"], [1596128725, "0.075125275730705"], [1596128735, "0.17515026320570498"], [1596128745, "0.425764260543"], [1596128755, "0.400877004686765"], [1596128765, "0.250563910071265"], [1596128775, "0.325676579058"], [1596128785, "0.35075180476650003"], [1596128795, "0.2506517548731"], [1596128805, "0.2504257387906"], [1596128815, "0.30060127785949997"], [1596128825, "0.225526240444415"], [1596128835, "0.20040080159859497"], [1596128845, "0.20043847092838"], [1596128855, "0.1503759398494"], [1596128865, "0.22541330792504"], [1596128875, "0.35072659112684"], [1596128885, "0.4007014533265"], [1596128895, "0.3003003003"], [1596128905, "0.22541338332"], [1596128915, "0.25043840835472997"], [1596128925, "0.225375751688355"], [1596128935, "0.225375751688355"], [1596128945, "0.225375751688355"], [1596128955, "0.225375751688355"]]
					}
				]
			}
		},
		mounted()
		{
			//parse initial date, Fill x_scale, y_scale with suitable values
			this.current_x = this.x;
			this.current_y = this.y;
			this.current_w = this.width;
			this.current_h = this.height;
		},
		methods:
			{

				onDrag: function (x, y)
				{
					//console.log(this.$parent.$el.parentElement.offsetLeft);
					//console.log(this.x + " " + x);
					this.current_x = x;
					this.current_y = y;
				},
				onResize: function (x, y, width, height)
				{

					console.log(x, y, width, height)

					this.current_x = x;
					this.current_y = y;
					this.current_w = width;
					this.current_h = height;

				},
			}

	}
</script>

<style>

	/* feel free to change anything in layout and / or styles (and code)*/

	.chartContainer {
		position: absolute;
		border: solid 1px gray;
	}

	.chart-tex-y-left {
		position: absolute;
		margin-top: 18px;
	}

	.chart-tex-y-left div {
		float: left;
		clear: left;
		text-align: right;
		/*margin-left: 10px;*/
		width: 50px;
		font-size: 13px;
		line-height: 60px;
		white-space: nowrap;
		color: #6b6f77
	}

	.chart-tex-x-bottom {
		position: absolute;
		width: 100%;
		height: 20px;
		bottom: 100px;
	}

	.chart-tex-x-bottom div {
		width: 114px;
		text-align: center;
		display: inline-block;
		font-size: 13px;
		line-height: 20px;
		color: #6b6f77
	}

	.legends_container {
		width: 100%;
		/*border: solid 1px blue;*/
		height: 80px;
		overflow-y: scroll;
		position: absolute;
		bottom: 0;
	}

	.one_legend {
		display: inline-block;
		padding: 3px;
		color: #a1a4a9;
		white-space: nowrap;
	}


	.legend-color {
		width: 16px;
		height: 4px;
		display: inline-block;
		margin-left: 10px;
	}

	.legend-text {
		display: inline-block;
		white-space: nowrap;
		margin-left: 10px;
	}

	.drag-handle {
		text-align: center;
		cursor: move;
		position: absolute;
		z-index: 1;
	}

	.image_canvas {
		position: absolute;
	}

	.image_canvas_overlay {
		position: absolute;
	}


</style>
