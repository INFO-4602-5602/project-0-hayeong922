# Information Visualization Project0


Collaborators:

	Linzi Xing
	Hayeong Song

##Project Specification:

Part 1: Interpreting Data (Done)

Part 2: Building Bars (Done)

Part 3: Building Scatterplots (Done)

Part 4: Interaction (Done)

When you click on the dot in the graph, the x-coordinate and y-coordinate will appear or will be printed in screen.
When hovering the graph, the dot will change it’s color to red and when the mouse is out the color changes back to it’s original color.

Part 5: Building Multiple Charts (Done)


##Bells & Whistles:

Bell: Tooltips (Done)

When hovering the dots that exist in graph, the tooltip will appear next to the dot that one is hovering to showing the x-coordinate and y-coordinate.

Bell: Xs and Ys (Done)

Bell: Styling your Visualization (not implemented)

Bell: Best Fit Lines (Done)

Whistle: Replication (Done)
Uploaded in github directory

For scatterplot, I used Tableau and compared to D3 it did not require coding. And I used csv file and set x to x-axis and y to y-axis. To represent scatterplot I used instead of 'measure' I used 'dimension' that way a dot can represnet value of x and y in csv file.

For bar chart, first, we separated the original csv data file into two, one contained number order and x value, the other contained number order and y value since we wanted to generate two figures corresponding to attributes x and y. Then we input the data files and set order and x, y vaules as dimensions. However, after that, the graph we could choose in 'show me' tag was just scatterplot, but we can choose marks' shape, so we set it as bar and got the bar charts we wanted. Compared with D3, sometimes Tableau can get me quite confused because even we've already had some dimensions and values, we need to change them to approriate formate and then it works. But it's quite fast to create some simple statistic plots without coding, which is convenient. 

Whistle: Coordinated Views (Done)



##Source code list:

1.SVG Text Element (https://www.dashingd3js.com/svg-text-element)

  Used to created text DOM to show the value over each bar.

Pieces referred: 

    var text = svgContainer.selectAll("text")
                        .data(circleData)
                        .enter()
                        .append("text");
	// the part above is used to create text DOM attached to the bars. 
	// the following part is used to set the text content and position.
    var textLabels = text
                 .attr("x", function(d) { return d.cx; })
                 .attr("y", function(d) { return d.cy; })
                 .text( function (d) { return "( " + d.cx + ", " + d.cy +" )"; })
                 .attr("font-family", "sans-serif")
                 .attr("font-size", "20px")
                 .attr("fill", "red");
		 

2.Will’s Block's D3 Scatterplot (Animations)(http://bl.ocks.org/WilliamQLiu/bd12f73d0b79d70bfbae)

  Used to implement Whistle: Transitions

Pieces referred:

	d3.select("h4")
                .on("click", function() { //when click button, different data will be loaded
                    var numValues = dataset.length; 
                    var maxRange = Math.random() * 1000;                      
		    	dataset = []; //initialize array to save the data
                    for(var i=0; i<numValues; i++) {
                        var newNumber1 = Math.floor(Math.random() * maxRange); 
                        var newNumber2 = Math.floor(Math.random() * maxRange); 
                        dataset.push([newNumber1, newNumber2]); 
                    }

                    //update the domains of scales
                    xScale.domain([0, d3.max(dataset, function(d) {
                        return d[0]; })]);
                    yScale.domain([0, d3.max(dataset, function(d) {
                        return d[1]; })]);

                    //update the dots position and color when click on certain button
                    svg.selectAll("circle")
                        .data(dataset) 
                        .transition() 
                        .duration(1000) 
                        .each("start", function() {  // Start animation
                            d3.select(this)  // get the point chosed
                                .attr("fill", "red")  
                                .attr("r", 5);  
                        })
                        .delay(function(d, i) {
                            return i / dataset.length * 500; 
                        })
                   	//set the new x and y values for dots
                        .attr("cx", function(d) {
                            return xScale(d[0]);  
                        })
                        .attr("cy", function(d) {
                            return yScale(d[1]);  
                        }
                        .each("end", function() {  //end animation
                            d3.select(this)  
                                .transition()
                                .duration(500)
                                .attr("fill", "black")  
                                .attr("r", 2);  
                        });

                        // Update X Axis
                        svg.select(".x.axis")
                            .transition()
                            .duration(1000)
                            .call(xAxis);

                        // Update Y Axis
                        svg.select(".y.axis")
                            .transition()
                            .duration(100)
                            .call(yAxis);
                });
		

3.regression.js from Tom Alexander(https://github.com/Tom-Alexander/regression-js/blob/master/build/regression.min.js)

  Used to import the linear regression function for Bell: Best Fit Lines

  The js file is covered to compute linear regression, code is too long, so doesn’t shown in readme file.


4.D3.js linear regression from stackoverflow(http://stackoverflow.com/questions/20507536/d3-js-linear-regression)

  Used to create the fitted lines for scatterplots in Part 5 ( Bell: Best Fit Lines )

Pieces referred:

	var max = d3.max(dataset, function (d) { return d.OvershootingSuperiore; }); 
	var myLine = svg.append("svg:line") 
			.attr("x1", x(0)) 
			.attr("y1", y(lr.intercept)) 
			.attr("x2", x(max)) 
			.attr("y2", y( (max * lr.slope) + lr.intercept )) 
			.style("stroke", "black"); 
	//After getting the parameters(slope and y intercept of the fitted line), we can draw the line based on the intercept point and the point with maximum x value.


5.Selecting elements of multiple bar charts at the same time on Google groups

(https://groups.google.com/forum/#!topic/d3-js/cYnedmP54f0)

Used to highlight the related pair of bars at the same time which is task Whistle: Coordinated Views.

Pieces referred:

	var bar1 = svg.selectAll(".bar1")  //we select the related bar DOM based on the same class.
		.data(data).enter()
	 	.append("rect")
	 	.attr("class", function (d, i) { return 'bar' + i; })
          
	var bar2 = svg.selectAll(".bar2")
		.data(data).enter()
	 	.append("rect")
	 	.attr("class", function (d, i) { return 'bar' + i; })

	d3.selectAll('rect').on('mouseover', function() {
    	var rectClass = d3.select(this).attr('class');
   
    	d3.selectAll('.' + rectClass)    //controling the bars based on class will be effective
        	.attr('opacity', 0.2);

6.Part 2: Building Bars, Part 3: Building Scatterplots, part 4: interaction part and Bell:tooltips were implemented by using the code taught in class (tutorial code). To be more specific, we reused some of the code and made changes according to certain tasks. 
	

