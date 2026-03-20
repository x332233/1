const express = require('express');
const cors = require('cors');
const bodyParser = require('body-parser');

const app = express();
const port = process.env.PORT || 3001;

// 中间件
app.use(cors({
  origin: '*', // 在生产环境中，应该设置具体的域名
  methods: ['GET', 'PUT', 'POST', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization']
}));
app.use(bodyParser.json());

// 内存存储数据（因为Render的文件系统是临时的）
let data = {
  pages: [
    {
      id: 'home',
      title: '首页',
      content: {
        hero: {
          title: '专业乡村自建房别墅解决方案',
          subtitle: '从设计到施工，从售后到维护，我们为您提供一站式服务',
          cta: '立即咨询'
        },
        services: [
          {
            icon: '📐',
            title: '别墅设计',
            description: '专业团队为您量身定制个性化别墅设计方案'
          },
          {
            icon: '🏗️',
            title: '施工建设',
            description: '严格按照设计图纸施工，确保工程质量'
          },
          {
            icon: '🛠️',
            title: '售后服务',
            description: '提供完善的售后维护服务，让您无后顾之忧'
          }
        ],
        advantages: [
          '专业设计团队',
          '严格质量控制',
          '透明价格体系',
          '完善售后服务'
        ]
      }
    },
    {
      id: 'about',
      title: '关于我们',
      content: {
        description: '四川善工建筑工程有限公司成立于2010年，是一家专注于乡村自建房别墅设计、施工、售后一站式服务的企业。我们拥有一支经验丰富的专业团队，致力于为客户打造高品质、个性化的住宅空间。',
        vision: '成为乡村自建房领域的标杆企业，为更多家庭提供优质的住宅解决方案',
        values: '诚信、专业、创新、共赢'
      }
    },
    {
      id: 'services',
      title: '服务内容',
      content: {
        items: [
          {
            title: '别墅设计',
            description: '根据客户需求和场地条件，提供个性化的别墅设计方案，包括建筑设计、结构设计、水电设计等。我们的设计团队会与客户充分沟通，确保设计方案符合客户的期望和预算。'
          },
          {
            title: '施工建设',
            description: '拥有专业的施工团队和先进的施工设备，严格按照设计图纸和施工规范进行施工。我们注重施工质量和安全，确保工程按时完成，达到客户满意的效果。'
          },
          {
            title: '材料采购',
            description: '与优质材料供应商建立长期合作关系，为客户提供质优价廉的建筑材料。我们会根据设计方案和客户预算，选择合适的材料，确保工程质量和成本控制。'
          },
          {
            title: '售后服务',
            description: '提供完善的售后维护服务，包括房屋维修、设备保养等。我们承诺在接到客户报修后24小时内响应，及时解决客户遇到的问题，让客户无后顾之忧。'
          }
        ]
      }
    },
    {
      id: 'cases',
      title: '成功案例',
      content: {
        items: [
          {
            id: 1,
            title: '成都双流现代别墅',
            area: '300㎡',
            style: '现代简约',
            image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=modern%20villa%20house%20in%20rural%20area%2C%20chinese%20style%2C%20high%20quality%20photography&image_size=landscape_16_9'
          },
          {
            id: 2,
            title: '眉山传统中式别墅',
            area: '280㎡',
            style: '传统中式',
            image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=chinese%20traditional%20villa%20house%2C%20rural%20area%2C%20high%20quality%20photography&image_size=landscape_16_9'
          },
          {
            id: 3,
            title: '乐山欧式别墅',
            area: '350㎡',
            style: '欧式',
            image: 'https://trae-api-cn.mchost.guru/api/ide/v1/text_to_image?prompt=european%20style%20villa%20house%2C%20rural%20area%2C%20high%20quality%20photography&image_size=landscape_16_9'
          }
        ]
      }
    },
    {
      id: 'contact',
      title: '联系我们',
      content: {
        info: {
          company: '四川善工建筑工程有限公司',
          phone: '028-12345678',
          email: 'info@scsgjz.online',
          address: '四川省成都市双流区华阳街道'
        }
      }
    }
  ]
};

// API路由

// 获取所有页面
app.get('/api/pages', (req, res) => {
  const data = readData();
  res.json(data.pages);
});

// 获取单个页面
app.get('/api/pages/:id', (req, res) => {
  const data = readData();
  const page = data.pages.find(p => p.id === req.params.id);
  if (page) {
    res.json(page);
  } else {
    res.status(404).json({ error: 'Page not found' });
  }
});

// 更新页面
app.put('/api/pages/:id', (req, res) => {
  const data = readData();
  const index = data.pages.findIndex(p => p.id === req.params.id);
  if (index !== -1) {
    data.pages[index] = { ...data.pages[index], ...req.body };
    writeData(data);
    res.json(data.pages[index]);
  } else {
    res.status(404).json({ error: 'Page not found' });
  }
});

// 启动服务器
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});
