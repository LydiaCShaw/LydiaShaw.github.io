Iteration Code
function Img = IR(FileName, IterNum)


AngNum = 128;
BinNum = 128;
ImgRow = 128;
ImgCol = 128;


fid = fopen(FileName, 'rt');
Sino1d = fscanf(fid,'%e ',inf);
fclose(fid);
figure
imshow(reshape(Sino1d, BinNum, AngNum)',[])


Img = ones(ImgRow * ImgCol, 1);
figure
for i =1:IterNum;
   
    ImgTmp =Mat * Img;
    ImgTmp = Sino1d ./ (ImgTmp + eps);
    
    ImgTmp = (ImgTmp'* Mat)';
    
  
    Img = Img .* ImgTmp ./ (sum(Mat)' + eps);   
    imshow(imresize(reshape(Img, ImgRow, ImgCol)',4), []);
    str = sprintf('Iteration # = %d', i);
    title(str)
    pause(0.1);
end


Img = reshape(Img, ImgRow, ImgCol)';
return;
