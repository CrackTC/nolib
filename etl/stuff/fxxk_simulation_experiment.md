```python
# -*- coding:utf-8 -*-
import requests
import urllib.parse
import datetime

uuid = input("uuid: ")
experimentId = input("experimentId: ")
courseId = input("courseId: ")

start_time = input("输入 start_time (YYYY-MM-DD hh:mm:ss): ")
if start_time == "":
    start_time = datetime.datetime.now() - datetime.timedelta(hours=1)
else:
    start_time = datetime.datetime.strptime(start_time, '%Y-%m-%d %H:%M:%S')

end_time = input("输入 end_time (YYYY-MM-DD hh:mm:ss): ")
if end_time == "":
    end_time = datetime.datetime.now()
else:
    end_time = datetime.datetime.strptime(end_time, '%Y-%m-%d %H:%M:%S')

timeUsed = str((end_time - start_time).seconds)

a = '{ "uuid": "' + uuid + '", "experimentId": ' + experimentId + ', "courseId": ' + courseId + ', "stepId": "", "taskId": "", "recordId": "", "platformType": 0, "startTime": ' + str(
    int(start_time.timestamp() * 1000)
) + ', "endTime": ' + str(
    int(end_time.timestamp() * 1000)
) + ', "timeUsed": ' + timeUsed + ', "status": 1, "score": 100.0, "appid": "", "secret": "", "steps": [], "ReportSummary": "", "reportModels": [ { "name": "产业振兴（得分：100分）", "reportContents": [ { "name": "理论题", "images": [], "imageType": "none", "type": "script", "inputs": {}, "script": "回首来时路，经过全党全国各族人民共同努力，我们完成了消除（______）的艰巨任务，我们党在团结带领人民创造美好生活、实现共同富裕的道路上迈出了坚实的一大步。同时，脱贫摘帽不是终点，而是新生活、新奋斗的起点。脱贫攻坚取得胜利后，接续全面推进乡村振兴，这是“三农”工作重心的历史性转移。巩固拓展（______）成果是全面推进（______）的前提。今年的政府工作报告指出，巩固拓展脱贫攻坚成果，坚决防止出现规模性返贫。\n输入答案：\n1、绝对贫困\n2、脱贫攻坚\n3、乡村振兴\n正确答案：\n1、绝对贫困\n2、脱贫攻坚\n3、乡村振兴\n得分：15\n在养殖肉牛的过程中，老乡们可能会遇到过哪些问题？（多选）\n选择的选项：ABC\n正确选项：ABC\n选项：\nA、肉牛长期粪便不成形\nB、肉牛病死率高\nC、经常丢失肉牛\n得分：5\n\n针对牛品种肉质差，市场价格也偏低，你有什么好办法？(多选)\n选择的选项：ABCD\n正确选项：ABCD\n选项：\nA、引进优质种牛精液，采用人工受精的方式，提升种牛品质，降低繁育生产成本。\nB、改良肉质，提高肉牛单产效益。\nC、拓宽销售渠道，如直播卖牛。\nD、牛肉深加工，增加牛肉产品种类。\n得分：5\n请将吉老师和志愿者们的内心独白和正在做的事情拖至对应的框内；（四项；每项5分）\n得分：20分", "datas": [] }, { "name": "操作步骤", "images": [], "imageType": "none", "type": "script", "inputs": {}, "script": "1.完成实地调研模块。（得分：10分）\n2.完成防盗系统研制模块。（得分：5分）\n3.完成改良肉牛质量，拓宽销售渠道模块。（得分：5分）1.完成实地调研模块。（得分：10分）\n2.完成防盗系统研制模块。（得分：5分）\n3.完成改良肉牛质量，拓宽销售渠道模块。（得分：5分）1.完成实地调研模块。（得分：10分）\n2.完成防盗系统研制模块。（得分：5分）\n3.完成改良肉牛质量，拓宽销售渠道模块。（得分：5分）", "datas": [] }, { "name": "三维交互操作", "images": [], "imageType": "none", "type": "script", "inputs": {}, "script": "1、牛场调研信息搜集（得分：15）\n误操作次数：0(总分：15分，每错一次扣除2分，最多扣5次)\n正确答案：\n1、肉牛数量:n个\n2、肉牛品种：品种1\n3、肉牛饲料：青储、干草、玉米秸秆、豆粕、玉米粉、钙、铁、锌、盐、苏打粉等等\n4、养殖场信息\n5、喂牛时间：凌晨5点一次，晚上17点一次2、防盗设备组件选择（得分：10分）\n误操作次数：0次（每次扣除1分，最多扣7次）\n正确答案：红外线传阵列感器，温度传感器，无线WiFi模块，gsm模块、开发板、上位机、摄像头\n3、手机组装（得分5分）", "datas": [] } ] }, { "name": "生态振兴（得分：100分）", "reportContents": [ { "name": "理论题", "images": [], "imageType": "none", "type": "script", "inputs": {}, "script": "那么，同学们知道盐碱地的成因吗？（多选）\n选择的选项：ABCDE\n正确选项：ABCDE\n选项：\nA、气候条件\nB、地理条件\nC、土壤质地和地下水\nD、河流和海水的影响\nE、耕作管理的不当\n得分：5\n\n需要记录在标签上的信息包括？（多选）\n选择的选项：ABC\n正确选项：ABC\n选项：\nA、日期\nB、地点\nC、土壤类型\nD、天气\n得分：5\n\n此次采集的土壤为？（单选）\n选择的选项：A\n正确选项：A\n选项：\nA、含有氯化物或硫酸盐较高的盐碱土地\nB、相对较为贫瘠的黄土地\nC、性状优良适宜耕种的黑土地\n得分：5\n\n此次采集的土壤为？（单选）\n选择的选项：B\n正确选项：B\n选项：\nA、含有氯化物或硫酸盐较高的盐碱土地\nB、相对较为贫瘠的黄土地\nC、性状优良适宜耕种的黑土地\n得分：5\n\n由于熟练度和环境等问题，衣服已经被蹭脏，我们应该怎么处理？（单选）\n选择的选项：A\n正确选项：A\n选项：\nA、继续努力完成任务，不耽误项目进度\nB、沟通其他同学换岗\nC、回去换衣服，不干了\nD、请求村民帮忙\n得分：5\n\n我国东北的黑土地是世界第（  ）大黑土区？（单选）\n选择的选项：C\n正确选项：C\n选项：\nA、一\nB、二\nC、三\nD、四\n得分：5\n\n黑土地是中国最肥沃的土地，那么黑土地是一种什么类型的资源？（单选）\n选择的选项：A\n正确选项：A\n选项：\nA、不可再生资源\nB、难以再生资源\nC、可再生资源\n得分：5\n\n通过对《中华人民共和国黑土地保护法》的学习，以下哪些行为，违反了黑土地保护法？（多选）\n选择的选项：ABCDEFG\n正确选项：ABCDEFG\n选项：\nA、非法盗挖黑土资源\nB、通过网络贩卖黑土\nC、将耕地用于非农化用途\nD、占用、滥用耕地\nE、对耕作层的土壤实施剥离\nF、滥用化肥，导致黑土地污染\nG、耕种和使用方式不当，造成水土流失\n得分：5\n\n选择合适的位置并张贴海报（多选）\n选择的选项：ABC\n正确选项：ABC\n选项：\nA、村部门口的宣传栏\nB、村子主干道两侧的宣传板\nC、村活动中心旁边的宣传栏\nD、村子内一户居民家的外墙上\nE、村口一棵粗壮大树的树干上\n得分：5", "datas": [] }, { "name": "操作步骤", "images": [], "imageType": "none", "type": "script", "inputs": {}, "script": "1.完成盐碱地见闻模块。（得分：10分）\n2.完成盐碱地改造模块。（得分：10分）\n1.完成盐碱地见闻模块。（得分：10分）\n2.完成盐碱地改造模块。（得分：10分）\n3.完成偶遇土地破坏模块。（得分：10分）", "datas": [] }, { "name": "三维交互操作", "images": [], "imageType": "none", "type": "script", "inputs": {}, "script": "任务一：收集粪便所需的工具选择（得分：5分）\n任务二：装载粪便（得分：2分）\n任务三：粪便运输至粪车上（得分：2分）\n任务四：秸秆打捆（得分：2分）\n任务五：秸秆运输至运输车上（得分：2分）\n农田交互：劳作前所需要的物品选择（得分：5）\n误操作次数：0(总分：5分，每错一次扣除2分，最多扣5次)\n正确答案：\n1）水鞋，2）防水裤，3）大檐帽，4）手套，在这些劳作工具中进行选择。\n水鞋与防水裤二选一，大檐帽与手套为必选项\n农田交互：抛秧间距点选择；（得分：5）\n误操作次数：0(总分：5分，每错一次扣除5分)\n找到并点击选择异常的土地；（得分：5）\n误操作次数：0(总分：5分，每错一次扣除5分)", "datas": [] } ] }, { "name": "文化振兴（得分：100分）", "reportContents": [ { "name": "理论题", "images": [], "imageType": "none", "type": "script", "inputs": {}, "script": "要推动乡村文化振兴，加强农村思想道德建设和公共文化建设，以社会主义核心价值观为引领，应该深入挖掘优秀传统农耕文化蕴含的（）？（多选）\n选择的选项：ABC\n正确选项：ABC\n选项：\nA、思想观念\nB、人文精神\nC、道德规范\n得分：5\n\n传承发展（ ）走乡村文化兴盛之路。（单选）\n选择的选项：C\n正确选项：C\n选项：\nA、商业文明\nB、现代文明\nC、农耕文明\n得分：5\n\n目前的基层文化现状是怎样的？（多选）\n选择的选项：ABC\n正确选项：ABC\n选项：\nA、群众自办文化热情高涨\nB、政府、社会共同合作\nC、依托互联网新科技\n得分：5\n\n在上述找到的东北特色中，选择属于非遗的物品（多选）\n选择的选项：DEF\n正确选项：DEF\n选项：\nA、炕\nB、花棉被\nC、搪瓷茶缸\nD、李锐士剪纸\nE、李向荣木版年画\nF、张玉欣布贴画\n得分：5\n\n要加大优质文化（）和服务供给，丰富农民群众的文化生活。（单选）\n选择的选项：C\n正确选项：C\n选项：\nA、精神\nB、内涵\nC、产品\n得分：5\n\n在经历了吉老师和木板画的经历之后，我们对基层文化建设有了更多的了解。那么，加强基层文化建设应该（）。（多选）\n选择的选项：ABCD\n正确选项：ABCD\n选项：\nA、文化内容供给要贴切、要优质、更要多元\nB、创新服务形式\nC、提高群众参与度满意度\nD、加大基层文化服务资金投入力度\n得分：5", "datas": [] }, { "name": "操作步骤", "images": [], "imageType": "none", "type": "script", "inputs": {}, "script": "1.完成调研当地实况，挖掘乡村文化资源模块。（得分：10分）\n2.完成了解非遗现状，培养乡村文化人才模块。（得分：5分）\n3.完成总结特色优势，发展乡村文化产业模块。（得分：5分）", "datas": [] }, { "name": "三维交互操作", "images": [], "imageType": "none", "type": "script", "inputs": {}, "script": "1、请在小王家中漫游；找到具有东北特色的物品（得分：20）\n误操作次数：0(总分：20分，每错一次扣除2分，最多扣5次)\n正确答案：\n1、炕\n2、花棉被\n3、搪瓷茶缸\n4、李锐士剪纸\n5、李向荣木版年画\n6、张玉欣布贴画\n2、请在村里找到合适的人员；发放宣传单（得分：15）\n误操作次数：0(总分：15分，每错一次扣除3分，最多扣5次)\n正确答案：\n1）家中有小孩子的妇人；\n2）15岁左右的青年；\n3）旅游的游客\n3、请找到并点击选择制作木版画的工具包（得分：10）\n误操作次数：0(总分：10分，每错一次扣除2分，最多扣5次)\n正确答案：\n刻板工具包、印刷工具包、裁纸工具包", "datas": [] } ] } ], "reportName": "" }'
data = urllib.parse.quote(a)
print(data)
url = 'https://virtualcourse.zhihuishu.com/report/saveReport'
headers = {
    'sec-ch-ua':
    '"Chromium";v="116", "Not)A;Brand";v="24", "Google Chrome";v="116"',
    'sec-ch-ua-platform': '"Linux"',
    'Referer': 'https://virtual-emulation.zhihuishu.com/',
    'sec-ch-ua-mobile': '?0',
    'User-Agent':
    'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/116.0.0.0 Safari/537.36',
    'Content-Type': 'application/x-www-form-urlencoded'
}

response = requests.post(url,
                         headers=headers,
                         data='jsonStr=' + data + '&ticket=')
print(response.text)
```
