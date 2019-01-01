# SWT2D.m

SWT2D.m

%% Synthesic data

%% 2D-SWT based Goyal & Kakkar(2016) algorithm for fused gravity & %magnetic data

%%Code by: Sara Nasri and Amin Roshandel kahoo

%%Tel:+98 23 3239 5509; Email address:roshandel@shahroodut.ac.ir

%Load Magnetic and Gravity data (Magnetic is image_new_1 and Gravity data  is image_new_2)

load image_new_1

load image_new_2

%%selection the desired section of the data

x1(:,:)=F(:,35,:); 

x2(:,:)=E(:,35,:);

X1=x1';

X2=x2';

%%Normalize data based on observed value and according to further fusion steps

X1=X1./max(abs(X1(:)));

X2=X2./max(abs(X2(:)));

%% made meshgrid for interpolation data, it is required especially for sparse data points and large data set

[X,Y]=meshgrid(1:70,1:30); 

[XX,YY]=meshgrid(1:.01:70,1:.01:30);

%%interpolation final models

image_new_1=interp2(X,Y,X1,XX,YY); 

image_new_2=interp2(X,Y,X2,XX,YY);

%%Normalize data based on observed value and according to further fusion steps

image_new_1=image_new_1./max(abs(image_new_1(:))); 

image_new_2=image_new_2./max(abs(image_new_2(:)));

%%Convert matrix to grayscale image

image_new_1=mat2gray(image_new_1); 

image_new_2=mat2gray(image_new_2);

image_new_1(:,end)=[];

image_new_1(1,:)=[];

image_new_2(:,end)=[];

image_new_2(1,:)=[];

%2-D wavelet decompose of input data in 3-level and got off it approximate and details coefficients (SWT).

[A1,H1,V1,D1] = swt2(image_new_1,1,'db1'); 

[A2,H2,V2,D2] = swt2(image_new_2,1,'db1');

A_swt = fusion_detail_2D(A1,A2,1);

H_swt = fusion_detail_2D(H1,H2,1);

V_swt = fusion_detail_2D(V1,V2,1);

D_swt = fusion_detail_2D(D1,D2,1);

%Multilevel 2-D wavelet reconstruction the using inversion of the stationary wavelet transform (ISWT).

X_swt = iswt2(A,H_swt,V_swt,D_swt,'db1'); 
