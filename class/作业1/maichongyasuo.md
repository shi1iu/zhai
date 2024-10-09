% 参数设置
fc = 1e9; % 载波频率
bw = 10e6; % 带宽
pulse_width = 10e-6; % 脉冲宽度
sampling_rate = 100e6; % 采样率

% 生成线性调频信号
t = 0 : 1/sampling_rate : pulse_width;
signal = exp(1i * 2 * pi * (fc * t + 0.5 * bw / pulse_width * t.^2));

% 添加噪声
noise = 0.1 * (randn(size(signal)) + 1i * randn(size(signal)));
received_signal = signal + noise;

% 频域脉冲压缩
S = fft(signal); % 发射信号的傅里叶变换
R = fft(received_signal); % 接收信号的傅里叶变换
compressed_signal_freq = R.* conj(S); % 频域相乘
compressed_signal = ifft(compressed_signal_freq); % 逆傅里叶变换得到时域脉冲压缩结果

% 绘制结果
figure;
subplot(3,1,1);
plot(real(signal));
title('发射信号实部');
subplot(3,1,2);
plot(real(received_signal));
title('接收信号实部');
subplot(3,1,3);
plot(abs(compressed_signal));
title('频域脉冲压缩结果');