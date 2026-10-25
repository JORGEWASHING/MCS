**EXP2** 
clc  ; 
 Pt  =  input  (  'Enter transmission power:'  )  ; 
 Gt  =  input  (  'Enter transmission gain antenna:'  )  ; 
 Gr  =  input  (  'Enter receive gain antenna:'  )  ; 
 f  =  input  (  'Enter frequency:'  )  ; 
 d  =  input  (  'Enter distance:'  )  ; 
 W  =  (  3  *  10  ^  8  )  /  f; 
 Pr  =  (  Pt  *  Gt  *  Gr  *  W  ^  2  )  /  (  4  *  3.142  *  d  )  ^  2 
 X1  =  sprintf  (  'Pr(%d)=%f'  ,d,Pr  )  ; 
 disp  (  X1  )  ; 
 X2  =  sprintf  (  'Pr(%s)=%f'  ,  'db'  ,  10  *  log10  (  Pr  ))  ; 
 X3  =  sprintf  (  'Pr(%s)=%d'  ,  'dbm'  ,  10  *  log10  (  Pr  /  0.001  ))  ; 
 disp  (  X2  )  ; 
 disp  (  X3  )  ; 
 d  =  1  :  1  :  30 
 f1  =  900  *  10  ^  6 
 f2  =  1800  *  10  ^  6 
 PL1  =  91.52  +  20  *  log10  (  d  )  ; 
 PL2  =  97.54  +  20  *  log10  (  d  )  ; 
 subplot  (  2  ,  1  ,  1  ) 
 plot  (  d,PL1  ) 
 title  (  'Path loss for 900MHz'  )  ; 
 xlabel  (  'distance(km)'  )  ; 
 ylabel  (  'pathloss(db)'  )  ; 
 subplot  (  2  ,  1  ,  2  ) 
 plot  (  d,PL2  ) 
 title  (  'Path loss for 1800MHz'  )  ; 
 xlabel  (  'distance(km)'  )  ; 
 ylabel  (  'pathloss(db)'  )  ; 
**exp4**
clc; 
clear; 
c = 3e8; 
fc = input("Enter Carrier Frequency fc (in Hz, e.g., 900e6): "); 
Vm_kmh = input("Enter Mobile Speed Vm (in km/h, e.g., 108): "); 
theta_deg = input("Enter Angle theta (in degrees, e.g., 60): "); 
Vm = Vm_kmh / 3.6; 
theta_rad = theta_deg * (%pi / 180); 
fd = (Vm * fc / c) * cos(theta_rad); 
fr = fc + fd; 
mprintf("\n================ RESULTS ================\n"); 
mprintf("Carrier Frequency (fc) : %.2f MHz\n", fc / 1e6); 
mprintf("Mobile Speed (Vm)      
mprintf("Angle (theta)          
 : %.2f km/h (%.2f m/s)\n", Vm_kmh, Vm); 
 : %.2f deg\n", theta_deg); 
mprintf("-----------------------------------------\n"); 
mprintf("Doppler Shift (fd)      
: %.2f Hz\n", fd); 
mprintf("Received Frequency (fr) : %.6f MHz\n", fr / 1e6); 
mprintf("=========================================\n"); 
theta_vec_deg = 0:1:180; 
theta_vec_rad = theta_vec_deg * (%pi / 180); 
fd_vec = (Vm * fc / c) * cos(theta_vec_rad); 
figure(1); 
plot(theta_vec_deg, fd_vec, 'b-', 'LineWidth', 2); 
plot(theta_deg, fd, 'ro', 'MarkerSize', 8, 'Thickness', 3); 
xtitle("Doppler Frequency Shift vs Angle of Arrival", "Angle theta (Degrees)", "Doppler Shift fd (Hz)"); 
xgrid(1); 
**exp6**
clc; clear; 
state = input("Enter initial state as a vector (e.g., [1 0 0 0]): "); 
n = length(state); 
N = (2^n) - 1; 
pn_seq = zeros(1, N); 
mprintf("\n--- STATE TABLE ---\n"); 
mprintf("Clock\tQ0\tQ1\tQ2\tQ3\tOutput (Q3)\n"); 
mprintf("--------------------------------------------\n"); 
for i = 1:N 
pn_seq(i) = state(4); 
mprintf("%d\t%d\t%d\t%d\t%d\t%d\n", i, state(1), state(2), state(3), state(4), pn_seq(i)); 
if state(3) == state(4) then 
feedback = 0; 
else 
feedback = 1; 
end 
state = [feedback, state(1:$-1)]; 
end 
mprintf("--------------------------------------------\n\n"); 
disp("Generated PN Sequence:"); 
disp(pn_seq); 
count_ones = sum(pn_seq == 1); 
count_zeros = sum(pn_seq == 0); 
mprintf("\n--- BIT COUNT SUMMARY ---\n"); 
mprintf("Total Number of 1s: %d\n", count_ones); 
mprintf("Total Number of 0s: %d\n", count_zeros); 
plot2d3(1:N, pn_seq, style=2); 
xtitle("Generated Pseudo-Noise (PN) Sequence", "Bit Index", "Binary Value"); 
a = gca(); 
a.data_bounds = [0, -0.2; N+1, 1.2]; 
**exp7**
clc;
clear;
close all;
Ad = 1;
if Ad == 1
    Ad_bipolar = 1;
else
    Ad_bipolar = -1;
end
Bd = 0;
if Bd == 1
    Bd_bipolar = 1;
else
    Bd_bipolar = -1;
end
codeA_binary = [0 0 1 1 0 0 1 1];
codeB_binary = [0 1 1 0 0 1 1 0];
codeA = 2 * codeA_binary - 1;
codeB = 2 * codeB_binary - 1;
As = Ad_bipolar * codeA;
Bs = Bd_bipolar * codeB;
Cs = As + Bs;
recA_product = Cs .* codeA;
sumA = sum(recA_product);
if sumA > 0
    rec_Ad = 1;
else
    rec_Ad = 0;
end
recB_product = Cs .* codeB;
sumB = sum(recB_product);
if sumB > 0
    rec_Bd = 1;
else
    rec_Bd = 0;
end
disp('--------------------------------------------------');
disp('                  CDMA SIMULATION                 ');
disp('--------------------------------------------------');
fprintf('Sender A Transmitted Data (Ad) : %d (Bipolar: %d)\n', Ad, Ad_bipolar);
fprintf('Sender B Transmitted Data (Bd) : %d (Bipolar: %d)\n\n', Bd, Bd_bipolar);
disp('Spread Signal A (As):');
disp(As);
disp('Spread Signal B (Bs):');
disp(Bs);
disp('Combined Channel Signal (Cs = As + Bs):');
disp(Cs);
disp('--------------------------------------------------');
fprintf('Receiver Decoding for Sender A:\n');
fprintf('  Sum Product = %d\n', sumA);
fprintf('  Decoded Data Ad = %d\n\n', rec_Ad);
fprintf('Receiver Decoding for Sender B:\n');
fprintf('  Sum Product = %d\n', sumB);
fprintf('  Decoded Data Bd = %d\n', rec_Bd);
disp('--------------------------------------------------');
figure;
subplot(3, 2, 1);
stem(codeA, 'LineWidth', 1.5, 'MarkerFaceColor', 'b');
title('Chip Code A (Bipolar)');
ylim([-2 2]); grid on;
subplot(3, 2, 2);
stem(codeB, 'LineWidth', 1.5, 'MarkerFaceColor', 'r');
title('Chip Code B (Bipolar)');
ylim([-2 2]); grid on;
subplot(3, 2, 3);
stem(As, 'LineWidth', 1.5, 'MarkerFaceColor', 'b');
title('Spread Signal A (As)');
ylim([-2 2]); grid on;
subplot(3, 2, 4);
stem(Bs, 'LineWidth', 1.5, 'MarkerFaceColor', 'r');
title('Spread Signal B (Bs)');
ylim([-2 2]); grid on;
subplot(3, 2, [5 6]);
stem(Cs, 'LineWidth', 2, 'MarkerFaceColor', 'k');
title('Combined Channel Signal (Cs = As + Bs)');
ylim([-3 3]); grid on;
**exp8**
clc; 
clear; 
close; 
m1=0; 
m2=0; 
m3=0; 
m4=0; 
m5=0; 
m6=0; 
m7=0; 
m8=0; 
m9=0;
a=input("Enter input bits (example [1 0 1 0]): "); 
n=length(a); 
a=[a zeros(1,8)]; 
N=length(a); 
v1=zeros(1,N); 
v2=zeros(1,N); 
c=1;
disp(" ");
disp("=============================================="); 
disp("          CONVOLUTIONAL ENCODER"); 
disp("=============================================="); 
mprintf("\nOriginal Input : "); 
for i=1:n 
    mprintf("%d",a(i)); 
end 
mprintf("\n"); 
mprintf("Input with zeros : "); 
for i=1:N 
    mprintf("%d",a(i)); 
end 
mprintf("\n\n"); 
disp("=============================================="); 
disp("              STATE TABLE"); 
disp("=============================================="); 
mprintf("\nStep   Input   Present State   V1 V2   Next State\n"); 
mprintf("--------------------------------------------------\n"); 
for j=1:N 
    mprintf("%2d       %d       ",j,a(j)); 
    mprintf("%d%d%d%d%d%d%d%d",m1,m2,m3,m4,m5,m6,m7,m8); 
    v1(j)=bitxor(m1,m2); 
    v1(j)=bitxor(v1(j),m3); 
    v1(j)=bitxor(v1(j),m4); 
    v1(j)=bitxor(v1(j),m6); 
    v1(j)=bitxor(v1(j),m8); 
    v1(j)=bitxor(v1(j),m9); 
    v2(j)=bitxor(m1,m3); 
    v2(j)=bitxor(v2(j),m4); 
    v2(j)=bitxor(v2(j),m5); 
    v2(j)=bitxor(v2(j),m9); 
    mprintf("       %d  %d     ",v1(j),v2(j)); 
    m9=m8; 
    m8=m7; 
    m7=m6; 
    m6=m5; 
    m5=m4; 
    m4=m3; 
    m3=m2; 
    m2=m1; 
    m1=a(j); 
    mprintf("%d%d%d%d%d%d%d%d\n",m1,m2,m3,m4,m5,m6,m7,m8); 
end 
mprintf("\n==============================================\n"); 
mprintf("                 OUTPUT\n"); 
mprintf("==============================================\n"); 
mprintf("\nV1 = "); 
for i=1:N 
    mprintf("%d",v1(i)); 
end 
mprintf("\n");
mprintf("V2 = "); 
for i=1:N 
    mprintf("%d",v2(i)); 
end
mprintf("\n"); 
// Final encoded output 
mprintf("\nFinal Encoded Output = "); 
for i=1:N 
mprintf("%d%d",v1(i),v2(i)); 
end 
mprintf("\n"); 
mprintf("\n==============================================\n"); 
mprintf("       
CONVOLUTIONAL ENCODING COMPLETED\n"); 
mprintf("==============================================\n");
**exp9**
function main() 
clc; 
clear; 
n = input("Enter matrix dimension (e.g., 2, 4, 8): "); 
N = log2(n); 
H = 1; 
for i = 1:N 
H = [H, H; H, -H]; 
end 
disp("Hadamard Matrix of size " + string(n) + "x" + string(n) + ":"); 
disp(H); 
h_changes = zeros(n, 1); 
for i = 1:n 
changes = 0; 
for j = 1:n-1 
if H(i, j) ~= H(i, j+1) then 
changes = changes + 1; 
end 
end 
h_changes(i) = changes; 
end 
disp("Number of sign changes for each Hadamard Matrix row:"); 
for i = 1:n 
disp("Row " + string(i) + ": " + string(h_changes(i)) + " changes"); 
end 
[w_changes, index] = gsort(h_changes, 'g', 'i'); 
W = H(index, :); 
disp("Walsh Code Matrix:"); 
disp(W); 
disp("Number of sign changes (Sequency) for each Walsh Code row:"); 
for i = 1:n 
disp("Row " + string(i) + ": " + string(w_changes(i)) + " changes"); 
end 
endfunction 
main(); 
