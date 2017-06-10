#概述

>JavaSound是一个小巧的低层API，支持数字音频(simpled-audio)和MIDI数据的记录/回放。在JDK 1.3.0之前，JavaSound是一个标准的Java扩展API，但从Java 2的1.3.0版开始，JavaSound就被包含到JDK之中。由于Java有着跨平台（操作系统、硬件平台）的特点，基于JavaSound的音频处理程序（包括本文的程序）能够在任何实现了Java1.3+的系统上运行，无需加装任何支持软件。

>当前JDK的JavaSound API随同Java媒体框架（JMF，Java Media Framework）一起发布，主页在java.sun.com/products/java-media/jmf/，适合JDK 1.1以及更高的版本。除了JDK实现的JavaSound API之外，还有一个源代码开放的JavaSound实现是Tritonus，主页在http://www.tritonus.org/。

>标准包是javax.sound.simpled和javax.sound.midi.第三方扩展包是javax.sound.simpled.spi和javax.sound.midi.spi(spi : service provider interface 服务提供接口)

#体系介绍
使用SOUND API播放声音至少需要3样东西:
* Mixer:混频器,将声音信息转化成数字信号,或者将数字信号转化成声音信号
* Line:传输音频数据并起到缓冲的作用,有两个重要的实现类SourceDataLine和TargetDataLine.sourceDataLine接受实时音频数据流,将音频输入混频器.targetdataline从混合器接收音频数据。
* formatted audio data:音频格式化信息
* AudioSystem:提供音频资源的访问服务


#示例
1.播放音乐
````
       try {  
            File file = new File("a15.mp3");  
            int offset = 0;  
            int bufferSize = Integer.valueOf(String.valueOf(file.length())) ;  
            byte[] audioData = new byte[bufferSize];  
            InputStream in = new FileInputStream(file);  
            in.read(audioData);  
            
            float sampleRate = 16000;  
            int sampleSizeInBits = 16;  
            int channels = 1;  
            boolean signed = true;  
            boolean bigEndian = false;  
         
            AudioFormat af = new AudioFormat(sampleRate, sampleSizeInBits, channels, signed, bigEndian);  
            SourceDataLine.Info info = new DataLine.Info(SourceDataLine.class, af, bufferSize);  
            SourceDataLine sdl = (SourceDataLine) AudioSystem.getLine(info);  
            sdl.open(af);  
            sdl.start();  
            while (offset < audioData.length) {  
                offset += sdl.write(audioData, offset, bufferSize);  
            }  
            in.close();
        } catch (LineUnavailableException e) {  
            e.printStackTrace();  
        } catch (FileNotFoundException e) {  
            e.printStackTrace();  
        } catch (IOException e) {  
            e.printStackTrace();  
        }  

````

2.从设备采集音频