<html>
<body>
<canvas id='Lab3' height='1000' width='1000'>
<script>
	var canvas = document.getElementById('Lab3');
	var ctx = canvas.getContext('2d');
	function blockMenu() {	
    		document.getElementById("Lab3").innerHTML = "Ia?aoa i?aaay eiiiea iuoe";
	}
	document.oncontextmenu = function()  { blockMenu(); return false; };
	function drawLine(x0, y0, x1, y1) {
		var deltax = Math.abs(x1 - x0);
		var deltay = Math.abs(y1 - y0);
		var error = 0;
		if (deltay <= deltax) {
			var deltaerr = deltay;
			var y = y0;
			if (x1 > x0) {
				for (var x = x0; x <= x1; ++x) {
					ctx.fillRect(x, y, 1, 1);
					error += deltaerr;
					if (2*error >= deltax) {
						if (y1 < y0) {
							y--;
						} else {
							y++;
						}
						error -= deltax;
					}
				}
			} else {
				for (var x = x0; x >= x1; --x) {
					ctx.fillRect(x, y, 1, 1);
					error += deltaerr;
					if (2*error >= deltax) {
						if (y1 < y0) {
							y--;
						} else {
							y++;
						}
						error -= deltax;
					}
				}
			}
		} else {
			var deltaerr = deltax;
			var x = x0;
			if (y1 > y0) {
				for (var y = y0; y <= y1; ++y) {
					ctx.fillRect(x, y, 1, 1);
					error += deltaerr;
					if (2*error >= deltay) {
						if (x1 < x0) {
							x--;
						} else {
							x++;
						}
						error -= deltay;
					}
				}
			} else {
				for (var y = y0; y >= y1; --y) {
					ctx.fillRect(x, y, 1, 1);
					error += deltaerr;
					if (2*error >= deltay) {
						if (x1 < x0) {
							x--;
						} else {
							x++;
						}
						error -= deltay;
					}
				}
			}
		}
	}
	function factorial(n) {
		return (n <= 1) ? 1 : n * factorial(n - 1);
	}
	function basis(i, n, t) {
		return (factorial(n)/(factorial(i)*factorial(n - i)))* Math.pow(t, i)*Math.pow(1 - t, n - i);
	}
	function linePoints(points, deltat) {
		var buf = new Array()	
		for (var t = 0; t < 1 + deltat; t += deltat) {
			var ind = buf.length;	
			buf[ind] = new Array(0, 0);	
			for (var i = 0; i < points.length; i++) {
				var b = basis(i, points.length - 1, t);		
				buf[ind][0] += points[i][0] * b;
				buf[ind][1] += points[i][1] * b;
			}
		}
		return buf;
	}
	function drawLines(arr) {
		for (var i = 0; i < arr.length - 1; ++i) {
			drawLine(arr[i][0],arr[i][1],arr[i+1][0],arr[i+1][1]);
		}
	}
	var x, y;		
	var points = new Array();
	var i = 0;
	canvas.onmousedown = function(event) {
		if (event.button == 0) {
			x = event.offsetX;
			y = event.offsetY;
			points[i] = new Array(x, y);
			i++;
		}
		else if (event.button == 2) {
			x = event.offsetX;
			y = event.offsetY;
			points[i] = new Array(x, y);
			drawLines(linePoints(points, 0.01));
			i = 0;
			points = [];
		}
	}
</script>
</canvas>
</body>
</html>
