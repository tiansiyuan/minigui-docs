# 新控件集Skin渲染器图片规格
# Appendix B Image Specification of Skin Renderer of the New Control Set



## 前言
## Foreword
- *skin的所有元素的类型都是文件名，该文件名对应的是加载到系统缓冲池中的图片(通过 LoadResource)。两者必须一致*
- skin renderer实际不保存文件名，只保存由这些文件名生成的RES_KEY,然后通过GetResource获取图片

- 由于很多控件有normal、press、highlight、diable等等状态的区分，对一个控件而言，每一个状态都保存成一幅图片，对资源的保存和图片加载的过程都是很大的浪费，所以mgncs中skin渲染器沿用minigui中的图片使用方式，就是将相关联的多张图合成一张，绘制时扣取不同的部分使用，例如，对button图片,我们设计成五个部分的组合图片，当我们要绘制某一个状态的button时，就取这一部分的子图来使用：%BR%
     <img src="%ATTACHURLPATH%/sample.png" alt="sample.png" width='104' height='234' />
- 图片的绘制方式有两种:
      $ %BLUE%直接拉伸填充绘制%ENDCOLOR%: 这种方式不难理解，就是把资源图片按照绘制区域的大小以一定的比例进行放大来填充。
      $ %BLUE%分段填充绘制%ENDCOLOR%: 这种方式会将取到的图片（或者子图）按照%RED%上、中、下%ENDCOLOR%或者%RED%左、中、右%ENDCOLOR%的方式分成三段，然后把两端单独填充，中间部分则循环使用用中间部分来进行填充，我们依然以button为例，如图%BR%
     <img src="%ATTACHURLPATH%/spl.png" alt="spl.png" width='367' height='98' />%BR%这种方式%RED%能较好得保留“圆角”等边界效果%ENDCOLOR%，使用这种方式填充的图片设计时要注意：%BR% 1. %RED%左中右、上中下3段的分界线每个图片是不同的，下面会详细说明%ENDCOLOR% %BR% 2. %RED%中间部分由于要循环使用，注意过渡平滑一些%ENDCOLOR%

- *Types of all the elements of skin are file name, and the file name corresponds to the image loaded to the system buffer pool (through LoadResource). The two must be consistent*
- renderer actually does not store file name, but only stores RES_KEY generated by these file names, and then gets image through GetResource

- Many controls have the distinction of status of normal, press, highlight, diable etc., for a control, each status is stored into an image, which is a huge waste for the process of storing resources and loading images, therefore, skin renderer in mGNCS continues to use the image use mode in minigui, which is mixing the associated images into one, and taking different parts to use when drawing. For example, for button image, we design into combination image of five parts, and when we want to draw button of certain status, we take child image of this part to use. %BR%
     <img src="%ATTACHURLPATH%/sample.png" alt="sample.png" width='104' height='234' />
- There are two drawing modes for the images:
      $ %BLUE%Direct stretch fill drawing%ENDCOLOR%: This mode is not difficult to understand, which is enlarging the resource image according to the size of the drawing region with certain proportion to fill.
      $ %BLUE%Segment fill drawing%ENDCOLOR%: This mode will divide the image (or child image) obtained into three segments according to the mode of %RED%up middle, down or left, middle right%ENDCOLOR%, and then  fill the two ends separately. For the middle part, use the middle part in circulation to fill. We still take button as the example, as shown in the figure
     <img src="%ATTACHURLPATH%/spl.png" alt="spl.png" width='367' height='98' />%BR%This mode %RED%can well reserve the border effect of "round corner" etc.%ENDCOLOR%, When using images filled in this mode to design, please not that: %BR% 1. %RED%1.	For the boundary line of left, middle, right and up, middle, down segments, the images are different, which will be explained below%ENDCOLOR% %BR% 2. %RED%Because the middle part will be used in circulation, note that transition shall be smooth%ENDCOLOR%


## 公用图片属性
## Property of the Public Image

| *属性名* | *类型* | *说明* |
| NCS_IMAGE_ARROWS | 文件名 | 箭头图片 |
| NCS_IMAGE_ARROWSHELL | 文件名 | 箭头按钮图片 |


| *Property name* | *Type* | *Explanation* |
| NCS_IMAGE_ARROWS | File name | Arrow image |
| NCS_IMAGE_ARROWSHELL | File name | Arrow button image |

- *图片的规格*
   - arrow图片用于skin渲染器中箭头的绘制，由自上而下的16部分小图构成,每个小图都是一个正方形，内部的对应关系是
         * 0~3     : 向上的箭头的各个状态（0-普通、1-高亮、2-按下、3-禁止）
         * 4~7     : 向下的箭头的各个状态（4-普通、5-高亮、6-按下、7-禁止）
         * 8~11   : 向左的箭头的各个状态（8-普通、9-高亮、10-按下、11-禁止）
         * 12~15 : 向右的箭头的各个状态（12-普通、13-高亮、14-按下、15-禁止）
   - arrow_shell图spin和scroll系列中，用于实现arrow按钮的效果，由自上而下的4部分小图构成,每个小图都是一个正方形，每一部分对应按钮的一种状态：
         * 0-普通
         * 1-高亮
         * 2-按下
         * 3-禁用
   - 另外，arrow图一般是配合arrow-shell图使用的，使用时有叠加的处理，所以arrow周围的区域一般做成透明的。
   - 使用该图片采用%BLUE%直接拉伸填充%ENDCOLOR%，注意图片合理设计。

- *Specification of the image*
   - arrow image is used for the drawing of arrows in the skin renderer, which is composed of 16 parts of small images from up to down, and each small image is a square. The internal corresponding relation is
         * 0-3     : statuses of the upward arrows (0-common, 1-hilight, 2-press down, 3-forbid)
         * 4-7     : Statuses of the downward arrows (4-common, 5-hilight, 2-press down, 3-forbid)）
         * 8-11   : statuses of the arrows leftward (8-common, 9-hilight, 10-press down, 11-forbid)
         * 12-15 : statuses of the rightward arrows (12-common, 13-hilight, 10-press down, 11-forbid)
   - In spin and scroll series of arrow_shell, effect used for realizing arrow button is composed of four parts of small images from up to down. Each small image is a square, and each part correspond to one status of the button:
         * 0-common
         * 1-hilight
         * 2-press down
         * 3-forbid
   - In addition, arrow image is generally used in cooperation with arrow-shell image. When in use, it has the superimposition effect, so region around arrow is generally made transparent.
   - Direct stretch filling is adopted when using the image, and pay attention to reasonable design of the image.

## mButton

| *属性名* | *类型* | *说明* |
| NCS_IMAGE_PUSHBUTTON | 文件名 | push button的图片 |

 *图片的规格*
- 用于skin渲染器中pushbutton的绘制效果，图片由自上而下的5部分组成，每一部分都是一个长方形的button，分别对应pushbutton的五种状态：
   - 0 - 普通
   - 1 - 高亮
   - 2 - 按下
   - 3 - 禁用
   - 4 - 三态按钮中的“半选”状态
- 绘制的时候，使用图片进行%BLUE%分段填充%ENDCOLOR%的，左右两端均为6个像素宽，中间一段宽度不限，注意图片的合理设计。%BR%


| *Property name* | *Type* | *Explanation* |
| NCS_IMAGE_PUSHBUTTON | File name | Image of push button |

 *Specification of image*
- For the drawing effect used for pushbutton in skin renderer, the image is composed of five parts from up to down, and each part is a rectangular button, corresponding to five statuses of pushbutton:
   - 0 - common
   - 1 - hilight
   - 2 - press down
   - 3 - forbid
   - 4 - “half selected” status in the three-status button
- When drawing, if image is used to carry out segmental filling, the left and right ends are 6 pixels wide, and width of the middle segment is not limited. Pay attention to reasonable design of the image.%BR%
     <img src="%ATTACHURLPATH%/btn.png" alt="btn.png" width='481' height='234' />

## mCheckbutton
| *属性名* | *类型* | *说明* |
| NCS_IMAGE_CHECKBUTTON | 文件名 | check button的图片 |

 *图片的规格*
- 用于skin渲染器的checkbutton的绘制渲染，图片由自上而下的8部分组成，每一部分都是一个长方形，分别对应checkbutton的8种状态：
   - 0~3:未选中时的普通、高亮、按下、禁用状态
   - 4~7:选中时的普通、高亮、按下、禁用状态
- 若图片大于绘制区域，会缩小图片进行绘制，否则采用%BLUE%直接用图片大小来填充%ENDCOLOR%，注意图片设计
- 示例%BR%

| *Property name* | *Type* | *Explanation* |
| NCS_IMAGE_CHECKBUTTON | File name | Image of check button |

 *Specification of image*
- For the drawing rendering used for checkbutton of skin renderer, the image is composed of eight parts from up to down. Each part is a rectangle, corresponding to eight statuses of checkbutton:
   - 0~3: common, hilight, press down and forbid status when not selected
   - 4~7: common, hilight, press down and forbid status when selected
- If the image is bigger than the drawing region, the image will be reduced to draw, otherwise %BLUE%image size is directly used to fill.%ENDCOLOR%, Pay attention to image design
- Example%BR%
     <img src="%ATTACHURLPATH%/skin_checkbtn.png" alt="skin_checkbtn.png" width='13' height='104' />

## mRadiobutton
| *属性名* | *类型* | *说明* |
| NCS_IMAGE_RADIOBUTTON |  文件名 | radio button的图片 |

 __图片的规格与checkbutton相同__

| *Property name* | *Type* | *Explanation* |
| NCS_IMAGE_RADIOBUTTON |  File name | Image of radio button |

 __Specification of the image is the same as checkbutton__

## mListView

| *属性名* | *类型* | *说明* |
| NCS_IMAGE_TREE | 文件名 | tree中折叠/展开开关的图片 |
| NCS_IMAGE_HEADER| 文件名 | 表头部分的图片 |

 *图片的规格*
- 用于实现树形控件中的折叠、展开按钮的渲染效果，图片由等份的上下两步分组成，上半部分是折叠的效果，下半部分是展开的效果
- 使用时，会直接以图片的大小来填充，不会拉伸，所以注意图片设计的尺寸，不要过大或过小
- 示例

| *Property name* | *Type* | *Explanation* |
| NCS_IMAGE_TREE | File name | Image of fold/unfold switch in the tree |
| NCS_IMAGE_HEADER| File name | Image of the header part |

 *Specification of the image*
- For the rendering effect used for realizing the fold and unfold button in the tree control, the image is composed of equal up and down parts. The up part is the fold effect, and the down part is unfold effect
- When in use, size of the image will be directly used to fill and it will not be stretched. So pay attention to the size of image design, and it shall not be too big or too small
- Example
     <img src="%ATTACHURLPATH%/skin_tree.png" alt="skin_tree.png" width='9' height='18' />

## mPropSheet

| *属性名* | *类型* | *说明* |
| NCS_IMAGE_TAB | 文件名 | PropSheet Tab页的图片 |

 *图片的规格*
- 用于渲染propsheet页的tab效果，由自上而下的四部分组成，mgncs中使用到的只有第一、三两部分，分别表示：
   - 0 - inactive页的tab效果
   - 2 - active页的tab效果
- %BLUE%分段绘制%ENDCOLOR%，左、中、右分别占3、x、4个像素，注意图片的设计
- 示例


| *Property name* | *Type* | *Explanation* |
| NCS_IMAGE_TAB | File name | Image of PropSheet Tab page |

 *Specification of image*
- For the tab effect used for rendering propsheet page, it is composed of four parts from up to down. In mGNCS, only the first part and the third part are used, representing:
   - 0 - tab effect of inactive page
   - 2 - tab effect of active page
- %BLUE%Draw by segment, %ENDCOLOR% left, middle and right occupy 3, x and 4 pixel respectively, and pay attention to the design of image
- Example
     <img src="%ATTACHURLPATH%/skin_tab.gif" alt="skin_tab.gif" width='18' height='72' />


## mProgressBar

| *属性名* | *类型* | *说明* |
| NCS_IMAGE_PB_HCHUNK | 文件名 | 水平ProgressBar的chunk图片 |
| NCS_IMAGE_PB_VCHUNK | 文件名 | 垂直ProgressBar的chunk图片  |
| NCS_IMAGE_PB_HSLIDER | 文件名 |  水平ProgressBar的trackbar图片 |
| NCS_IMAGE_PB_VSLIDER |  文件名 | 垂直ProgressBar的trackbar图片 |

 *图片的规格*
- h_chunk 
   - 用于水平进度条的进度显示，使用上block风格的progressbar分块拉伸填充，smooth风格直接拉伸填充
   - 图片设计上建议不要水平渐变
   - 示例


| *Property name* | *Type* | *Explanation* |
| NCS_IMAGE_PB_HCHUNK | File name | chunk image of horizontal ProgressBar |
| NCS_IMAGE_PB_VCHUNK | File name | chunk image of vertical ProgressBar  |
| NCS_IMAGE_PB_HSLIDER | File name |  trackbar image of horizontal ProgressBar |
| NCS_IMAGE_PB_VSLIDER |  File name | trackbar image of vertical ProgressBar |

 *Specification of image*
- h_chunk 
   - Used for the progress display of horizontal progress bar. Use progressbar block of up block style to stretch fill, and for smooth style, directly stretch fill 
   - Horizontal gradual change is not suggested in image design
   - Example
     <img src="%ATTACHURLPATH%/skin_pb_htruck.png" alt="skin_pb_htruck.png" width='10' height='12' />
- v_chunk 
   - 用于竖直进度条的进度显示，使用上block风格的progressbar分块拉伸填充，smooth风格直接拉伸填充
   - 图片设计上建议不要竖直渐变
   - Example

- v_chunk 
   - Used for progress display of vertical progress bar. Use progressbar of up block style to stretch fill, and for smooth style, directly stretch fill
   - Vertical gradual change is not suggested in image design
   - Example
     <img src="%ATTACHURLPATH%/skin_pb_vtruck.png" alt="skin_pb_vtruck.png" width='12' height='10' />
- h_slider
   %RED%未使用%ENDCOLOR%
- h_slider
   %RED%未使用%ENDCOLOR%

- h_slider
   %RED%is not used%ENDCOLOR%
- h_slider
   %RED%is not used%ENDCOLOR%

## mTrackbar

| *属性名* | *类型* | *说明* |
| NCS_IMAGE_TB_HSLIDER | 文件名 | 水平Trackbar的Slider 图片|
| NCS_IMAGE_TB_VSLIDER | 文件名 | 垂直Trackbar的Slider 图片|
| NCS_IMAGE_TB_HTHUMB | 文件名 | 水平Trackbar的Trackbar 图片|
| NCS_IMAGE_TB_VTHUMB | 文件名 | 垂直Trackbar的Trackbar 图片|

 *图片的规格*
- h_slider
   - 用于水平trackbar的滑轨渲染，图片作为整体来使用，
   - 使用时采用分段填充处理，左右两段各为两个像素宽,中间部分循环使用，图片设计注意。
   - Example
     <img src="%ATTACHURLPATH%/skin_tbslider_h.gif" alt="skin_tbslider_h.gif" width='5' height='5' />
- v_slider
   - 用于竖直trackbar的滑轨渲染，图片作为整体来使用，
   - 使用时采用分段填充处理，上下两段各为两个像素宽,中间部分循环使用，图片设计注意。
   - 示例


| *Property name* | *Type* | *Explanation* |
| NCS_IMAGE_TB_HSLIDER | File name | Slider image of horizontal Trackbar|
| NCS_IMAGE_TB_VSLIDER | File name | Slider image of vertical Trackbar|
| NCS_IMAGE_TB_HTHUMB | File name | Trackbar image of horizontal Trackbar|
| NCS_IMAGE_TB_VTHUMB | File name | Trackbar image of vertical Trackbar|

 *Specification of image*
- h_slider
   - Used for slide rail rendering of horizontal trackbar, and the image is used as a whole,
   - When in use, segmental fill processing is adopted. The left and right segments are two pixels wide respectively, and the middle part is used in circulation. Pay attention to image design.
   - Example
     <img src="%ATTACHURLPATH%/skin_tbslider_h.gif" alt="skin_tbslider_h.gif" width='5' height='5' />
- v_slider
   - Used for slide rail rendering of vertical trackbar, and the image is used as a whole,
   - When in use, segmental fill processing is adopted. The left and right segments are two pixels wide respectively, and the middle part is used in circulation. Pay attention to image design.
   - Example
     <img src="%ATTACHURLPATH%/skin_tbslider_v.gif" alt="skin_tbslider_v.gif" width='5' height='5' />
- h_thumb
   - 用于水平trackbar的滑块渲染，图片有自上而下均分的四部分组成，分别对应滑块的普通、高亮、按下、禁用四种状态使用
   - 图片使用时直接拉伸填充，注意图片设计
   - Example%BR%

- h_thumb
   - Slide block rendering used for horizontal trackbar. The image is composed of four parts equally from up to down, corresponding to the use of common, hilight, press down and forbid statuses of the slide block
   - When the image is used, directly stretch fill, and pay attention to image design
   - Example%BR%
     <img src="%ATTACHURLPATH%/skin_tb_horz.gif" alt="skin_tb_horz.gif" width='21' height='44' />
- v_thumb
   - 用于竖直trackbar的滑块渲染，图片有自上而下均分的四部分组成，分别对应滑块的普通、高亮、按下、禁用四种状态使用
   - 图片使用时直接拉伸填充，注意图片设计
   - 示例%BR%

- v_thumb
   - Used for slide rendering of vertical trackbar. The image is composed of four parts equally from up to down, corresponding to the use of common, hilight, press down and forbid statuses of the slide block
   - When the image is used, directly stretch fill, and pay attention to image design
   - Example%BR%
     <img src="%ATTACHURLPATH%/skin_tb_vert.gif" alt="skin_tb_vert.gif" width='11' height='84' />


-- Main.XiaodongLi - 06 Mar 2010