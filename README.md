//暴力
double Method(Node p[], int length)
{
	if (length == 1)		//点数为1输出无穷大
		return MAX_DISTANCE;
	else if (length == 2)	//点数为2直接输出点距
		return ((p[0].x - p[1].x)*(p[0].x - p[1].x) + (p[0].y - p[1].y)*(p[0].y - p[1].y));
	else{
		int i, j;
		double mindis, temp;
		mindis = (p[0].x - p[1].x)*(p[0].x - p[1].x) + (p[0].y - p[1].y)*(p[0].y - p[1].y);	//先取前两个点的点距为最小距离
		minPoint1 = p[0];
		minPoint2 = p[1];
		for (i = 0; i < length - 1; i++){
			for (j = i + 1; j < length; j++){
				temp = (p[i].x - p[j].x)*(p[i].x - p[j].x) + (p[i].y - p[j].y)*(p[i].y - p[j].y);	//算出每两个点的点距
				if (temp < mindis){				{
					mindis = temp;			//当发现更小的距离时，替换最小点距，并记录两个点的信息
					minPoint1 = p[i];
					minPoint2 = p[j];
				}
			}
		}
		return mindis;			//直到返回结果时才求sqrt得到真正距离，减少运算量
	}
}
