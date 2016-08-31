## 关于matlab GUI学习笔记
观看[21世纪电子论坛](http://bbs.21eic.com)《10分钟学习Matlab GUI系列》

### 1 GUI对象的基础概念及操作
- get 获得对象属性
- set 设置某对象属性
- findobj 寻找符合属性要求的对象
- allchild 寻找某一对象的子对象

### 2 GUI对象操作实例

```m
%% 创建界面
h = figure('Units','Normalize',...
    'position',[0.2 0.2 0.5 0.5],...
    'Menu','none');

ha = axes('Parent',h,'Units','Normalized',...
    'Position',[0.1 0.1 0.8 0.8]);

h1 = line('Parent',ha,'XData',[0:0.01:6],...
    'YData',sin([0:0.01:6]),'Color','r');

cstring = 'gbkmy';
while 1
    for k=1:length(cstring)
        set(h1,'Color',cstring(k));
        pause(3);
    end
end
```

### 3 底层代码实现GUI
- Figure
- Axes
- Line
- Text
- Uicontrol
```m
hf=figure(...
    'Units','Normalized',...
    'Menu','none',...
    'Color','w',...
    'Position',[0.2 0.2 0.6 0.5]);

ha=axes(...
    'Parent',hf,...
    'Units','Normalized',...
    'Position',[0.1 0.1 0.6 0.8],...
    'Box','on',...
    'NextPlot','Add');


hb1=uicontrol(...
    'Parent',hf,...
    'Units','Normalized',...
    'Style','pushbutton',...
    'Callback','plot(sin([0:0.01:6]),''g'')',...
    'String','sin',...
    'Units','Normalized',...
    'Position',[0.75 0.2 0.15 0.1]);

hb2=uicontrol(...
    'Parent',hf,...
    'Units','Normalized',...
    'Style','pushbutton',...
    'Callback','plot(cos([0:0.01:6]),''r'')',...
    'String','cos',...
    'Units','Normalized',...
    'Position',[0.75 0.4 0.15 0.1]);

hb3=uicontrol(...
    'Parent',hf,...
    'Units','Normalized',...
    'Style','pushbutton',...
    'Callback','try,delete(allchild(ha));end',...
    'String','clear',...
    'Units','Normalized',...
    'Position',[0.75 0.6 0.15 0.1]);
```
### 4 常用对象及属性1
- figure对象
```m
hf=figure;
get(hf)
% 改变颜色
set(hf,'Color','w');
set(hf,'Menubar','none')
set(hf,'NumberTitle','off','Name','演示');
set(hf,'Resize','off');
pause(3)
set(hf,'Visible','off');
pause(3)
set(hf,'Visible','on');
set(hf,'WindowStyle','modal');
set(hf,'WindowKeyPressFcn','Closereq');
set(hf,'WindowButtonDownFcn','Closereq');
hb=uicontrol('Style','pushbutton','Callback','closereq');
```
- axes对象
```m
ha=axes;
get(ha)
set(ha,'NextPlot','add');
plot([0:100]);
plot(sin(0:0.01:3));
set(ha,'NextPlot','replace');
```

### 5.常用对象及属性2
- line对象
```m
hf=figure;
hl=plot([0:10]);
get(hl)
set(hl,'Color','r',...
    'Marker','p',...
    'MarkerEdgeColor','g',...
    'MarkerFaceColor','k');
% 绘制sin(x)
hl1=ezplot('sin(x)');
x=get(hl1,'XData');
y=get(hl1,'YData');
figure
plot(x,y)
```
### 6.常用对象及属性3
- text对象
```m
ha=axes;
ht=text(1,1,'示例');
get(ht)
text('String','\int_0^x dF(x)','Position',[0.5 0.5]);
text('Interpreter','latex','String',...
'$$ \int_0^x dF(x) $$','Position',[0.2 0.2]);
plot(x);
% 在原始语句两边加上单引号
'plot(x);'
%当原始语句中含有引号，那么将原始的单引
%号都改为两个单引号，然后在最外层加上一对单引号
'plot(x,y,''r'');'

```
### 7.GUI对话框
- uigetfile文件打开对话框函数
```m 
%[FileName,PathName,FilterIndex] = uigetfile(FilterSpec)
% 规定打开文件类型
uigetfile('*.m');
% 输出参数意义 [文件名,路径,取消还是确定了]
[a,b,c]=uigetfile('*.m');
[a,b,c]=uigetfile('*.txt');
if c==1
    load(fullfile(b,a));
end
uigetfile('*.m','实例','default.m');
```
- uiputfile文件保存对话框函数
```m 
doc uiputfile
[a,b,c]=uiputfile('.m');
```
### 8.GUI颜色设置，字体设置对话框
- uisetcolor 颜色设置对话框
```m 
c=uisetcolor
c=uisetcolor([1 0 0]);
h=plot([0:10]);
c=uisetcolor(h);
figure
b=uicontrol('Parent',gcf,'String','颜色设置','Style','pushbutton',...
    'Callback','c=uisetcolor;set(b,''BackgroundColor'',c);');
```
- uisetfont字体设置对话框函数 
```m
uisetfont
doc uisetfont
S=uisetfont(b);
figure;
b=uicontrol('Parent',gcf,'String','颜色设置','Style','pushbutton',...
    'Callback','uisetfont(b);','Position',[0.2 0.2 0.8 0.8],...
    'Units','Normalized');
```
### 9.进度条对话框
- waitbar
```m 
h=waitbar(0,'实例');
get(h)
% 获得进度条的子对象
get(get(h,'Children'))
ha=get(h,'Children');
% 获得坐标轴子对象的子对象内容
get(ha,'Children')
get(ans(1))
get(ans(2))
hrand=waitbar(0.3,'颜色')
ha1=get(hrand,'Children');
hac=get(ha1,'Children');
hapa=findall(hac,'Type','patch')
set(hapa,'FaceColor','k')
doc waitbar
waitbar(0.5,hrand)
```
### 10.普通对话框、错误对话框、警告对话框
- dialog普通对话框
```m 
h=dialog('name','关于...','Position',[200 200 200 70]);
uicontrol('Parent',h,'Style','pushbutton','Position',[80 10 50 20],...
    'String','确定','Callback','delete(gcbf)');
```
- errordlg 错误对话框
```m
errordlg
```
- warndlg 警告对话框
```m 
warndlg
```

### 11. 输入对话框、 目录选择对话框 、 列表选择对话框
- inputdlg 输入对话框
```m 
name=inputdlg('请输入姓名','实例');
ret=inputdlg({'请输入姓名','请输入性别'},'实例');
info=inputdlg('请留言','留言',5);
re=inputdlg({'请输入姓名','请输入性别'},'实例',1,{'hui','男'},'on');
```
- uigetdir 目录选择对话框
```m 
uigetdir('C:\','浏览');
```
- listdig 列表选择对话框
```m 
[Sel,OK]=listdlg(...
    'ListString',{'A','B','C','D'},...
    'OKString','确定',...
    'CancelString','取消',...
    'Name','选择',...
    'SelectionMode','single');
```
### 12. GUIDE 的使用
- 对象拖进舞台
- 添加菜单
- 对象的排列
- tab键设置
- callback 函数中通过handels结构体获取属性`get(handles.pushbutton,'String')`
- guidata GUI数据通过handels.tag结构体访问
- 句柄值tag handles.tag

### 13.GUI 数据的一致访问
- 在 Open函数中加入自定义对象
```m 
function GUI13_OpeningFcn(hObject, eventdata, handles, varargin)
handles.output = hObject;

h=uicontrol('Tag','push','Callback',{@push_Callback,handles});
handles.push=h;

% Update handles structure
guidata(hObject, handles);
```
- 添加对应push_Callback函数
```m 
function push_Callback(hObject,eventdata,handles)
set(handles.edit1,'String',num2str(handles.rand));
```
### 14. GUI 示例
- Open函数中加入自定义对象
```m
function GUI14_OpeningFcn(hObject, eventdata, handles, varargin)
handles.output = hObject;
handles.x=-pi:0.01:pi;%添加x值，便于访问
guidata(hObject, handles);
```
- Clear_Callback的示例
```m 
function Clear_Callback(hObject, eventdata, handles)
try
    delete(allchild(handles.plotarea));
end
```
### 15.GUI 菜单
- Callback函数的书写
- 用visible 属性的设置控制对象的可见性


### 16.GUI 右键菜单
- 注意设置UIContexMenu属性选择对象的右键菜单，默认None
- 底层代码实现右键菜单
```m 
figure('Menubar','none');
h=uicontextmenu;
uimenu(h,'Label','A');
uimenu(h,'Label','B');
set(gcf,'Uicontextmenu',h);
```

### 17.带有时间显示的GUI
matlab 编程支持多线程，支持定时
- ExecutionMode 属性 
- Period 定是周期
- TimerFcn 时间到达后的操作
```m 
function GUI17_OpeningFcn(hObject, eventdata, handles, varargin)
handles.output = hObject;
handles.ht=timer;
set(handles.ht,'ExecutionMode','FixedRate',...
    'Period',1,...
    'TimerFcn',{@dispnow,handles});%调用函数，handels结构体做输入
start(handles.ht);
```

- 定义dispnow函数
```m 
function dispnow(hObject,eventdata,handles)
set(handles.disptime,'String',datestr(now));
```

### 18.界面响应鼠标事件
```m 
function GUI18_OpeningFcn(hObject, eventdata, handles, varargin)
handles.output = hObject;
global ButtonDown pos1;
ButtonDown=[];
pos1=[];
guidata(hObject, handles);
```
```m 
function figure1_WindowButtonDownFcn(hObject, eventdata, handles)
global ButtonDown pos1;
if strcmp(get(gcf,'SelectionType'),'normal')
    ButtonDown=1;
    pos1=get(handles.axes1,'CurrentPoint');
end
```
```m 
function figure1_WindowButtonMotionFcn(hObject, eventdata, handles)
global ButtonDown pos1;
if ButtonDown==1;
    pos=get(handles.axes1,'CurrentPoint');
    line([pos1(1,1) pos(1,1)],[pos1(1,2) pos(1,2)],'LineWidth',4);
    pos1=pos;
end
```

```m 
function figure1_WindowButtonUpFcn(hObject, eventdata, handles)
global ButtonDown
ButtonDown=0;
``` 
### 19 20.界面响应键盘事件、界面修饰
- KeyPressFcn 回调函数
- CurrentCharacter
```m 
    delete(gcf);%关闭窗口
% 如果按下enter 执行内容
if double(get(gcf,'CurrentCharacter'))==13
    Load_Callback(hObject, eventdata, handles)
end

```
- CData属性设置背景
```m 
% 在Open函数中设置
A = imread('按钮.jpg');
set(handles.pushbutton,'CData',A);

```
