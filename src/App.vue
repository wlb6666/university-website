<script setup>
import { computed, ref } from 'vue'

const imageUrl = (filename) => `${import.meta.env.BASE_URL}images/${filename}`

const navigation = [
  { label: '首页', category: '', children: [] },
  { label: '学校概况', category: '学校概况', children: ['学校简介', '校园风光', '大学精神'] },
  { label: '组织机构', category: '组织机构', children: ['理事会', '监事会', '党群结构'] },
  { label: '党建思政', category: '', children: [] },
  { label: '教学科研', category: '', children: ['本科教育', '专科教育', '教学科研'] },
  { label: '产教融合', category: '', children: ['校企合作', '国际交流'] },
  { label: '招生就业', category: '', children: ['招生网', '就业网'] },
  { label: '师资队伍', category: '', children: ['师资概况', '人才招聘'] },
]

const pageContents = {
  学校简介: { category: '学校概况', kind: 'intro' },
  校园风光: {
    category: '学校概况',
    kind: 'gallery',
    images: [
      { src: imageUrl('FG1.jpg'), alt: '校园风光一' },
      { src: imageUrl('FG2.jpg'), alt: '校园风光二' },
    ],
  },
  大学精神: {
    category: '学校概况',
    kind: 'spirit',
    heading: '三好一专 十一工程',
    points: [
      '做一个诚实守信的人，每天写一篇日记；',
      '做一个健康快乐的人，每天锻炼一小时；',
      '做一个聪明智慧的人，掌握一门知识管理方法；',
      '做一个善于沟通的人，参加一个社团组织；',
      '做一个勤于学习的人，每月读一本好书；',
      '做一个富有团队精神的人，参加一个项目实践；',
      '做一个具有国际竞争力的人，掌握一门外语；',
      '做一个 IT 时代的人，掌握一系列计算机和网络技能；',
      '做一个掌握职业技能的人，有一本职业资格证书或职称；',
      '做一个具有综合就业能力的人，掌握一系列就业技巧和本领。',
    ],
  },
  理事会: {
    category: '组织机构',
    paragraphs: [
      '理事长：陈苏',
      '副理事长：饶勇',
      '理事：庄力可、范红铭、陈方平、周明华、熊虎',
    ],
  },
  监事会: {
    category: '组织机构',
    paragraphs: ['监事会：汤胜红', '监事：张永革、谢文平'],
  },
  党群结构: {
    category: '组织机构',
    paragraphs: [
      '党委组织部（党校）、党委教师工作部、人事处（教师发展中心）',
      '党委宣传部（党委政治安全部）',
      '党委统战部（督导专员办）、工会、妇女联合会、关心下一代工作委员会办公室',
      '纪检监察室',
      '党委学生工作部（学生工作处）、团委',
    ],
  },
}

const currentPage = ref('学校简介')
const currentPageContent = computed(() => pageContents[currentPage.value])
const isSchoolCategory = computed(() => currentPageContent.value.category === '学校概况')
const sideMenu = computed(() =>
  isSchoolCategory.value
    ? navigation.find((item) => item.category === '学校概况').children
    : navigation.find((item) => item.category === '组织机构').children,
)

function changeContent(pageName) {
  if (pageContents[pageName]) {
    currentPage.value = pageName
  }
}
</script>

<template>
  <header class="top-header">
    <div class="header-container">
      <a class="logo-area" href="#" aria-label="江西软件职业技术大学首页">
        <img
          :src="imageUrl('logo.png')"
          alt="江西软件职业技术大学"
          class="logo-img"
        />
      </a>

      <nav class="main-nav" aria-label="主导航">
        <ul class="nav-list">
          <li v-for="item in navigation" :key="item.label" :class="{ 'has-dropdown': item.children.length }">
            <a
              href="#"
              :class="{ active: item.category === currentPageContent.category }"
              @click.prevent="item.category ? changeContent(item.children[0]) : null"
            >
              {{ item.label }}
            </a>
            <ul v-if="item.children.length" class="dropdown">
              <li v-for="child in item.children" :key="child">
                <a href="#" @click.prevent="changeContent(child)">{{ child }}</a>
              </li>
            </ul>
          </li>
        </ul>
      </nav>

      <div class="header-tools">
        <span class="campus-tag">江西软件职业技术大学(井冈山)</span>
        <span class="search-icon" aria-label="搜索">🔍</span>
      </div>
    </div>
  </header>

  <section class="banner-section" aria-label="校园横幅"></section>

  <main class="main-content">
    <div class="breadcrumbs">
      当前位置：{{ currentPageContent.category }} / {{ currentPage }}
    </div>

    <div class="content-wrapper">
      <aside class="sidebar" aria-label="侧边导航">
        <button
          v-for="item in sideMenu"
          :key="item"
          type="button"
          class="menu-item"
          :class="{ active: currentPage === item }"
          @click="changeContent(item)"
        >
          {{ item }}
        </button>
      </aside>

      <article class="article-content">
        <h1 class="article-title">| {{ currentPage }}</h1>
        <div class="article-body">
          <div class="content-section">
            <div v-if="currentPageContent.kind === 'intro'" class="school-intro">
              <img :src="imageUrl('ahead.jpg')" alt="江西软件职业技术大学校园" class="school-intro-image" />
              <div class="school-intro-text">
                <p>江西软件职业技术大学是全国第一所软件职业技术大学，由国家软件百强企业先锋软件股份有限公司于2003年创办，成立之初就成功入选全国35所示范性软件职业技术学院。2018年升格为职业本科学校，成为全国首批15所本科层次职业教育高校之一。2019年5月教育部批准更名为江西软件职业技术大学。2022年6月，学校获批职业本科教育学士学位授予单位。2024年7月，教育部正式批复同意江西软件职业技术大学新增办学地址，成立井冈山校区。</p>
                <p>目前，学校南昌校区和井冈山校区占地面积共1960亩，建筑面积65万平方米。学校设置有人工智能学院、区块链学院、软件工程学院、网络工程学院、数字经济学院、数智艺术学院、电子信息学院、马克思主义学院、国际教育学院、退役军人教育学院、创新创业学院、继续教育学院等12个学院，全日制在校生19613人；专任教师1158人，其中具有高级专业技术职务教师占比41%，具有硕士及以上学位教师占比61%，“双师型”教师占比64%。现开设25个职业本科专业、8个高职专科专业。建有国家体育产业示范项目电子竞技人才实训基地、职业本科虚拟仿真示范实训基地、北斗通航重点实验室等实验实训基地（室）。</p>
                <p>学校全面贯彻党的教育方针，始终坚持高等职业教育的属性和软件企业办软件大学的特色，秉承“创业创新、知识管理”校训，牢记“学好软件、报效祖国”使命，坚持“校企融合、产学互动”办学模式，实践“三好一专、十一工程”人才培养体系，凝练了学习的先锋、运动的先锋、快乐的先锋、创新的先锋、劳动的先锋“五个先锋”校园文化育人品牌，形成了“用企业家育人理念培养学生”“请工程师当老师”“拿真实的项目进行实训”的校企深度融合发展路径。</p>
                
              </div>
            </div>
            <div v-else-if="currentPageContent.kind === 'gallery'" class="campus-gallery">
              <img
                v-for="image in currentPageContent.images"
                :key="image.src"
                :src="image.src"
                :alt="image.alt"
                class="campus-gallery-image"
              />
            </div>
            <section v-else-if="currentPageContent.kind === 'spirit'" class="spirit-content">
              <h2 class="spirit-heading">{{ currentPageContent.heading }}</h2>
              <ol class="spirit-list">
                <li v-for="point in currentPageContent.points" :key="point">{{ point }}</li>
              </ol>
            </section>
            <template v-else>
              <p v-for="paragraph in currentPageContent.paragraphs" :key="paragraph">
                {{ paragraph }}
              </p>
            </template>
          </div>
        </div>
      </article>
    </div>
  </main>
</template>
