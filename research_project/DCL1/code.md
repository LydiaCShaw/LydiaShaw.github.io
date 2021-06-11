#Iteration Code
%输入迭代次数和1D弦图
%输出重建图像
function Img = IR(FileName, IterNum)

%投影角度数
AngNum = 128;

%每个角度下平行投影个数
BinNum = 128;

%图像行数
ImgRow = 128;

%图像列数
ImgCol = 128;

%读取弦图（即投影数据）
fid = fopen(FileName, 'rt');
Sino1d = fscanf(fid,'%e ',inf);
fclose(fid);

figure
imshow(reshape(Sino1d, BinNum, AngNum)',[])

%引入系统矩阵（稀疏）
load Mat128_Na128.mat

%迭代初始值
Img = ones(ImgRow * ImgCol, 1);

figure
for i =1:IterNum;
    %正投影
    ImgTmp =Mat * Img;
    
    ImgTmp = Sino1d ./ (ImgTmp + eps);
    
    %反投影
    ImgTmp = (ImgTmp'* Mat)';
    
    %获得新的迭代图像
    Img = Img .* ImgTmp ./ (sum(Mat)' + eps);   
   
    %逐步显示迭代图像
    imshow(imresize(reshape(Img, ImgRow, ImgCol)',4), []);
    str = sprintf('Iteration # = %d', i);
    title(str)
    pause(0.1);
end

%1D图像转化成2D图像
Img = reshape(Img, ImgRow, ImgCol)';

return;
