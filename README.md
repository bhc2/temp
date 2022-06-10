
<!DOCTYPE html>
<html>
    <head>
        <title>Programs</title>
    </head>
    <body>
        <strong> 3d serpenski (7th program)</strong>
    <pre>
#include&ltstdlib.h>
#include&ltstdio.h>
#include&ltGL/glut.h>
typedef GLfloat point[3];
point v[]={{-1.0,-0.5,0.0},{1.0,-0.5,0.0},{0.0,1.0,0.0}, {0.0,0.0,1.0}};
GLfloat colors[4][3]={{1.0,0.0,0.0},{0.0,1.0,0.0},{0.0,0.0,1.0},{1.0,1.0,0.0}};
int n;
void triangle(point a,point b,point c)
{
glBegin(GL_POLYGON);
glVertex3fv(a);
glVertex3fv(b);
glVertex3fv(c);
glEnd();
}

void tetra(point a,point b,point c,point d)
{
glColor3fv(colors[0]);
triangle(a,b,c);
glColor3fv(colors[1]);
triangle(a,c,d);
glColor3fv(colors[2]);
triangle(a,d,b);
glColor3fv(colors[3]);
triangle(b,d,c);
}

void divide_tetra(point a,point b,point c,point d,int m)
{
point mid[6];
int j;
if(m>0)
{
for(j=0;j< 3; j++)
{
mid[0][j]=(a[j]+b[j])/2.0;
mid[1][j]=(a[j]+c[j])/2.0;
mid[2][j]=(a[j]+d[j])/2.0;
mid[3][j]=(b[j]+c[j])/2.0;
mid[4][j]=(c[j]+d[j])/2.0;
mid[5][j]=(b[j]+d[j])/2.0;
}
divide_tetra(a,mid[0],mid[1],mid[2],m-1);
divide_tetra(mid[0],b,mid[3],mid[5],m-1);


divide_tetra(mid[1],mid[3],c,mid[4],m-1);
divide_tetra(mid[2],mid[5],mid[4],d,m-1);
}
else
tetra(a,b,c,d);
}
void display()
{
glClear(GL_COLOR_BUFFER_BIT|GL_DEPTH_BUFFER_BIT);
glClearColor(1.0,1.0,1.0,1.0);
divide_tetra(v[0],v[1],v[2],v[3],n);
glFlush();
}

void myReshape(int w,int h)
{
glViewport(0,0,w,h);
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
if(w<=h)
glOrtho(-1.0,1.0,-1.0*((GLfloat)h/(GLfloat)w), 1.0*((GLfloat)h/(GLfloat)w),-1.0,1.0);
else
glOrtho(-1.0*((GLfloat)w/(GLfloat)h),1.0*((GLfloat)w/(GLfloat)h),-1.0,1.0,-1.0,1.0);
glMatrixMode(GL_MODELVIEW);
glutPostRedisplay();
}

void main(int argc,char ** argv)
{
printf( "No of Division?: ");
scanf("%d",&n);
glutInit(&argc,argv);
glutInitDisplayMode(GLUT_SINGLE|GLUT_RGB|GLUT_DEPTH);
glutInitWindowSize(500,500);
glutCreateWindow( "3D gasket" );
glutDisplayFunc(display);
glutReshapeFunc(myReshape);
glEnable(GL_DEPTH_TEST);
glutMainLoop();
}
</pre>
<br>
<strong> Bresenham's Line Drawing (1st Program)</strong>
<pre>

    #include &ltGL/glut.h>
#include &ltstdio.h>
int x1, y1, x2, y2;

void myInit() 
{
    glClearColor(0.0, 0.0, 0.0, 1.0);
    glMatrixMode(GL_PROJECTION);
    gluOrtho2D(0, 500, 0, 500);
}

void draw_pixel(int x, int y) 
{
    glBegin(GL_POINTS);
    glVertex2i(x, y);
    glEnd();
}
void draw_line(int x1, int x2, int y1, int y2) 
{
    int dx, dy, i, e;
    int incx, incy, inc1, inc2;
    int x,y;
    dx = x2-x1;
    dy = y2-y1;
    if (dx < 0) dx = -dx;
    if (dy < 0) dy = -dy;
    incx = 1;
    if (x2 < x1) incx = -1;
    incy = 1;
    if (y2 < y1) incy = -1;
    x = x1; y = y1;
    if (dx > dy)
    {
        	draw_pixel(x, y);
        	e = 2 * dy-dx;
       		inc1 = 2*(dy-dx);
        	inc2 = 2*dy;
        		for (i=0; i< dx; i++)
		 {
            	 	if (e >= 0)
			{
 y += incy;e += inc1;
}
             		else
                 		 e += inc2;
           			 x += incx;
               		 draw_pixel(x, y);
       	     	   }
    } 
   else 
   {
        draw_pixel(x, y);
        e = 2*dx-dy;
        inc1 = 2*(dx-dy);
        inc2 = 2*dx;
    	    for (i=0; i< dy; i++) 
	   {
         		 if (e >= 0)
         		 {
 		x+= incx;
	    		e += inc1;
 }
 else
 e += inc2;
y += incy;
draw_pixel(x, y);
}
}
}

void myDisplay() 
{
glClear(GL_COLOR_BUFFER_BIT);    
draw_line(x1, x2, y1, y2);
glFlush();
}

int main(int argc, char **argv)
{
printf( "Enter end points of the Line (x1, y1, x2, y2)\n");
scanf("%d %d %d %d", &x1, &y1, &x2, &y2);
glutInit(&argc, argv);
glutInitDisplayMode(GLUT_SINGLE|GLUT_RGB);
glutInitWindowSize(500, 500);
glutInitWindowPosition(0, 0);
glutCreateWindow("Bresenham's Line Drawing");
myInit();
glutDisplayFunc(myDisplay);
glutMainLoop();
return 0;
}
</pre>

<br>
<strong> Triangle Rotation (2nd Program) </strong>
<pre>
    #define BLACK 0
    #include&ltstdio.h>
    #include&ltmath.h>
    #include&ltGL/glut.h>
    
    GLfloat house[3][3]={{100.0,250.0,175.0},{100.0,100.0,300.0},{1.0,1.0,1.0}};
    GLfloat rotatemat[3][3]={{0},{0},{0}};
    GLfloat result[3][3]={{0},{0},{0}};
    GLfloat arbitrary_x=0;
    GLfloat arbitrary_y=0;
    float rotation_angle;
    
    void multiply()
    {
    int i,j,k;
    for(i=0;i< 3;i++)
    for(j=0;j< 3;j++)
    {
    result[i][j]=0;
    for(k=0;k< 3;k++)
result[i][j]=result[i][j]+rotatemat[i][k]*house[k][j];
}
}


void rotate()
{
GLfloat m,n;
m=-arbitrary_x*(cos(rotation_angle) -1) + arbitrary_y * (sin(rotation_angle));
n=-arbitrary_y * (cos(rotation_angle) - 1) -arbitrary_x * (sin(rotation_angle));
rotatemat[0][0]=cos(rotation_angle);
rotatemat[0][1]=-sin(rotation_angle);
rotatemat[0][2]=m;
rotatemat[1][0]=sin(rotation_angle);
rotatemat[1][1]=cos(rotation_angle);
rotatemat[1][2]=n;
rotatemat[2][0]=0;
rotatemat[2][1]=0;
rotatemat[2][2]=1;
//multiply the two matrices
multiply();
}

void drawhouse()
{
glColor3f(0.0, 0.0, 1.0);
glBegin(GL_LINE_LOOP);
glVertex2f(house[0][0],house[1][0]);
glVertex2f(house[0][1],house[1][1]);
glVertex2f(house[0][2],house[1][2]);
glEnd();
}

void drawrotatedhouse()
{
glColor3f(1.0, 0.0, 0.0);
glBegin(GL_LINE_LOOP);
glVertex2f(result[0][0],result[1][0]);
glVertex2f(result[0][1],result[1][1]);
glVertex2f(result[0][2],result[1][2]);
glEnd();
}
void display()
{
glClear(GL_COLOR_BUFFER_BIT);
drawhouse();
drawrotatedhouse();
glFlush();
}

void myinit()
{
glClearColor(1.0,1.0,1.0,1.0);
glColor3f(1.0,0.0,0.0);
glPointSize(1.0);
glMatrixMode(GL_PROJECTION);
glLoadIdentity();
gluOrtho2D(0.0,499.0,0.0,499.0);
}

int main(int argc, char** argv)
{
int ch;
printf("Enter your choice \n1: Rotation about origin \n2: Rotation about a Fixed point\n");
scanf("%d",&ch);

switch(ch)
   	{
		case 1: printf("Enter the rotation angle in degree :");
	   	scanf("%f", &rotation_angle);
            	rotation_angle= (3.14 * rotation_angle) / 180;
	   	rotate();
        	   	break;

   		case 2: printf("Enter the fixed points :");
	   	scanf("%f%f", &arbitrary_x,&arbitrary_y);
         	 	printf("Enter rotation angle in degree :");
	   	scanf("%f", &rotation_angle);
           		rotation_angle= (3.14 * rotation_angle) / 180;
	   	rotate();
	   	break;

   	}
glutInit(&argc,argv);
glutInitDisplayMode(GLUT_SINGLE|GLUT_RGB);
glutInitWindowSize(500,500);
glutInitWindowPosition(0,0);
glutCreateWindow("house rotation");
glutDisplayFunc(display);
myinit();
glutMainLoop();
return 0;
}

</pre>
        
            
    </body>
    </html>
