<template lang="">
    <div class="main">
        <div class="intro">
            <h4>Park Minji</h4>
            <h1>Portfolio</h1>
            <div class="round">
                <p class="pointer">
                    <span @click="point(project)">Project</span>
                    <span @click="point(about)">About</span>
                    <span @click="point(contact)">Contact</span>
                </p>
                <p class="default">
                    <span>Frontend</span>
                    <span>Developer</span>
                </p>
            </div>
        </div>
        <div class="project" id="project">
            <h1>Project</h1>
            <div class="p_contant">
                <Swiper
                    :modules="modules"
                    :slides-per-view="'auto'"
                    :direction="'horizontal'"
                    :space-between="20"
                    :scrollbar="true"
                    :mousewheel="true"
                    class="project-swiper"
                >
                    <SwiperSlide
                        v-for="(project, index) in projects" 
                        :key="index" 
                        @mouseover="hoverState[index] = true"
                        @mouseleave="hoverState[index] = false"
                    >
                        <div class="p_card">
                            <div class="card">
                                <!-- 카드 표지 -->
                                <div v-if="!hoverState[index]" class="card_img">
                                    <img :src="project.image" alt="Project image"/>
                                </div>
                                <!-- 카드 내용 -->
                                <div v-if="hoverState[index]" class="card_contant"
                                    @mouseover="isHovered = true" 
                                    @mouseleave="isHovered = false"
                                >
                                    <h3>{{ project.name }}</h3>
                                    <p>{{ project.description }}</p>
                                    <p>{{ project.technologies }}</p>
                                    <div class="c_icon">
                                        <div class="p_icon" @click="ProjectLink(project.projectLink)"><img :src="project.projectIcon" alt="Project icon"/></div>
                                        <span/>
                                        <div class="g_icon" @click="ProjectLink(project.githubLink)"><img :src="project.githubIcon" alt="GitHub icon"/></div>
                                    </div>
                                </div>
                            </div>
                            <h3 class="card_name">{{ project.name }}</h3>
                        </div>
                    </SwiperSlide>
                </Swiper>
            </div>
        </div>
        <div class="about" id="about">
            <h1>About</h1>
            <div class="introduce">
                <p>
                    어떤 문제라도 열린 마음으로 접근하고, 다양한 아이디어를 바탕으로 효율적으로 해결책을 모색하며, 팀과 함께 나아갑니다.
                </p>
            </div>
            <div class="key">
                <div class="key_btn">
                    <div
                    v-for="(keyword, index) in keywords" :key="index" 
                    :class="['btn',{ active: realIndex == index }]" @click="slideTo(index)">
                        <div class="S_img"><img :src="keyword.iconS"/></div>
                        <span>{{ keyword.label }}</span>
                    </div>
                </div>
                <div class="key_word">
                    <swiper
                        @swiper="setSwiperRef"  
                        :direction="'vertical'"
                        :slidesPerView="3"
                        :spaceBetween="100"
                        :mousewheel="true"
                        :centeredSlides="true"
                        :modules="modules"
                        class="mySwiper"
                        @slideChange="onSlideChange"
                    >
                        <swiper-slide
                        v-for="(keyword, index) in keywords" :key="index">
                            <div class="word">
                                <div class="B_img"><img :src="keyword.iconB"/></div>
                                <div class="kw">
                                    <span>{{ keyword.hashtag }}</span>
                                    <p>{{ keyword.description }}</p>
                                </div>
                            </div>
                        </swiper-slide>
                    </swiper>
                </div>
            </div>
            <!-- 모바일 탭 버튼 -->
            <div class="tab">
                <div class="tab_button">
                    <button
                        v-for="(keyword, index) in keywords"
                        :key="index"
                        :class="{ active: activeTab === index }"
                        @click="activeTab = index"
                    >
                        <img :src="keyword.iconS"/>
                    </button>
                </div>
                <div class="tab_content">
                    <div v-for="(keyword, index) in keywords" :key="index">
                        <div v-if="activeTab === index" class="tab_text">
                            <dt>{{ keyword.hashtag }}</dt>
                            <dd>{{ keyword.description }}</dd>
                        </div>
                    </div>
                </div>
            </div>                
        </div>
        <div class="contact" id="contact">
            <h1>Contact</h1>
            <div class="conta">
                <div @click="copyText('mjpark.2k@gmail.com')">
                    <div class="conta_img"><img src="contact/email_B.icon.svg"/></div>
                    <!-- <h3>mjpark.2k@gmail.com</h3> -->
                    <h3>E-mail</h3>
                </div>
                <span/>
                <div @click="openGithubLink">
                    <div class="conta_img"><img src="contact/github_B.icon.svg"/></div>
                    <!-- <h3>github.com/mjpark-2k</h3> -->
                    <h3>Github</h3>
                </div>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent, ref, Ref } from 'vue';
import { Swiper, SwiperSlide } from 'swiper/vue';
import 'swiper/css';
import 'swiper/css/pagination';
import { Mousewheel, Pagination } from 'swiper/modules';

interface Project {
  name: string;
  description: string;
  technologies: string;
  image: string;
  projectIcon: string;
  githubIcon: string;
  projectLink: string;
  githubLink: string;
  isHovered: boolean;
}

interface Keyword {
  label: string;
  hashtag: string;
  description: string;
  iconS: string;
  iconB: string;
}

export default defineComponent({
  name: 'YourComponentName',
  components: {
    Swiper,
    SwiperSlide,
  },
  setup() {
    const project = ref('project');
    const about = ref('about');
    const contact = ref('contact');

    const slides = ref<string[]>(
      Array.from({ length: 4 }).map((_, index) => `Slide ${index + 1}`)
    );

    const swiperRef: Ref<any> = ref(null);
    const realIndex = ref<number>(0);

    const setSwiperRef = (swiper: any) => {
      swiperRef.value = swiper;
    };

    const slideTo = (index: number) => {
      realIndex.value = index;
      swiperRef.value?.slideTo(index);
    };

    const onSlideChange = (e: any) => {
      realIndex.value = e.activeIndex;
    };

    const hoverState = ref<boolean[]>(Array(4).fill(false));

    const projects = ref<Project[]>([
      {
        name: '해양생물연구센터',
        description: '팀프로젝트 작업 \n 노후화 된 사이트 리뉴얼 \n 인사말, 오시는 길, 구조, 보호 페이지 제작',
        technologies: 'html, css, scss, javascript',
        image: 'project/teamB_B.icon.svg',
        projectIcon: 'project/teamB_S.icon.svg',
        githubIcon: 'project/github_S.icon.svg',
        projectLink: 'https://mingz-x.github.io/B/',
        githubLink: 'https://github.com/mingz-x/B.git',
        isHovered: false,
      },
      {
        name: 'Sema 서울시립미술관',
        description: '서울열린데이터광장 제공 \n 서울시 오픈 API 사용',
        technologies: 'Html, Css, Javscript, \n Next.js, Firebase, Nextauth',
        image: 'project/sema_B.icon.svg',
        projectIcon: 'project/sema_S.icon.svg',
        githubIcon: 'project/github_S.icon.svg',
        projectLink: 'https://sema-iota.vercel.app',
        githubLink: 'https://github.com/mingz-x/sema.git',
        isHovered: false,
      },
      {
        name: 'Movie',
        description: '영화 오픈 API 사용',
        technologies: 'Html, Css, Javscript, React.js',
        image: 'project/movie_B.icon.svg',
        projectIcon: 'project/movie_S.icon.svg',
        githubIcon: 'project/github_S.icon.svg',
        projectLink: 'https://movie-theta-black.vercel.app',
        githubLink: 'https://github.com/mingz-x/movie.git',
        isHovered: false,
      },
      {
        name: 'News',
        description: '뉴스 오픈 API 사용',
        technologies: 'Html, Css, Javscript, Vue.js',
        image: 'project/news_B.icon.svg',
        projectIcon: 'project/news_S.icon.svg',
        githubIcon: 'project/github_S.icon.svg',
        projectLink: 'https://news-sable-one.vercel.app',
        githubLink: 'https://github.com/mingz-x/news.git',
        isHovered: false,
      },
      {
        name: 'Portfolio',
        description: '포트폴리오 사이트',
        technologies: 'Html, Css, Javscript, Vue.js',
        image: 'project/portfolio_B.icon.svg',
        projectIcon: 'project/portfolio_S.icon.svg',
        githubIcon: 'project/github_S.icon.svg',
        projectLink: 'https://portfolio-minjis-projects-b5399ada.vercel.app',
        githubLink: 'https://github.com/mingz-x/portfolio.git',
        isHovered: false,
      },
    ]);

    const keywords = ref<Keyword[]>([
      {
        label: '긍정적',
        hashtag: '# 긍정적인',
        description: '긍정적인 성격으로 문제 해결과 협업 등 팀워크에서 긍정적인 에너지를 발산합니다.',
        iconS: 'about/positive_S.svg',
        iconB: 'about/positive_B.svg',
      },
      {
        label: '분석적',
        hashtag: '# 분석적인',
        description: '차분하게 데이터를 분석하고 꼼꼼하게 작업하여 코드의 품질을 높입니다.',
        iconS: 'about/analytical_S.svg',
        iconB: 'about/analytical_B.svg',
      },
      {
        label: '창의적',
        hashtag: '# 창의적인',
        description: '창의적 사고로 혁신적인 해결책을 제시합니다.',
        iconS: 'about/creative_S.svg',
        iconB: 'about/creative_B.svg',
      },
      {
        label: '의사소통',
        hashtag: '# 의사소통',
        description: '다양한 팀원들과의 협업에서 효과적인 의사소통 능력을 구사합니다.',
        iconS: 'about/communication_S.svg',
        iconB: 'about/communication_B.svg',
      },
    ]);

    const activeTab = ref<number>(0);

    const point = (id: string) => {
      const pointnav = document.getElementById(id);
      if (pointnav) {
        pointnav.scrollIntoView({ behavior: 'smooth', block: 'start' });
      }
    };

    const ProjectLink = (url: string) => {
      window.open(url, '_blank');
    };

    const copyText = (text: string) => {
      navigator.clipboard.writeText(text)
        .then(() => alert('주소가 복사되었습니다!'))
        .catch(() => alert('복사에 실패했습니다.'));
    };

    const openGithubLink = () => {
      window.open('https://github.com/mjpark-2k', '_blank');
    };

    return {
        project,
        about,
        contact,
        slides,
        swiperRef,
        realIndex,
        setSwiperRef,
        slideTo,
        onSlideChange,
        hoverState,
        projects,
        keywords,
        activeTab,
        point,
        ProjectLink,
        copyText,
        openGithubLink,
        modules: [Pagination, Mousewheel],
    };
  },
});
</script>

<style lang="scss">
$mobile: 767px;
$tablet: 768px;
$pc: 1200px;

    .main{
        max-width: 1440px;
        margin: 0 auto;
        img{vertical-align: middle;}
        .intro{
            height: 100vh;
            align-content: center;
            h4{
                margin: 0;
                font-size: 5em;  /* 80px */
                /* 6.25em = 100px */
                cursor: default;
                @media (max-width: $mobile) {
                    font-size: 40px;
                }
            }
            h1{
                margin: 0;
                margin-bottom: 20px;
                font-size: 12.5em;  /* 200px */
                cursor: default;
                @media (min-width: $tablet) and (max-width: $pc) {
                    font-size: 150px;
                }
                @media (max-width: $mobile) {
                    font-size: 70px;
                }
            }
            .round{
                p{
                    margin: 10px 0;
                    span{
                        display: inline-block;
                        width: 264px;
                        margin: 5px 10px;
                        padding: 7px 20px;
                        text-align: center;
                        border: 2px solid black;
                        border-radius: 25px;
                        font-size: 1.875em; /* 30px */
                        @media (min-width: $tablet) and (max-width: $pc) {
                            width: 222px;
                            padding: 5px 0;
                        }
                        @media (max-width: $mobile) {
                            width: 250px;
                            font-size: 20px;
                            padding: 0;
                        }
                    }
                }
                .pointer{
                    span{
                        cursor: pointer;
                        @media (max-width: $mobile) {
                            background-color: black;
                            color: white;
                        }
                    }
                    span:hover{
                        background-color: black;
                        color: white;
                        @media (max-width: $mobile) {
                            background-color: white;
                            color: black;
                        }
                    } 

                }
                .default{
                    cursor: default;
                }
            }
        }
        h1{
            font-size: 6.25em; /* 100px */
            cursor: default;
            margin-bottom: 150px;
            @media (min-width: $tablet) and (max-width: $pc) {
                font-size: 80px;
            }
            @media (max-width: $mobile) {
                font-size: 55px;
                margin-bottom: 80px;
            }
        }
        p{
            word-break: keep-all;
        }
        .project{
            height: 100vh;
            
            .swiper-slide{
                width: fit-content;
            }
            
            margin: 300px 0;
            @media (max-width: $mobile) {
                margin: 0;
            }
            .p_contant{
                margin: 150px 100px;
                display: flex;
                flex-direction: row;
                flex-wrap: nowrap;
                gap: 20px;
                justify-content: flex-start;
                @media (max-width: $mobile) {
                    margin: 80px 20px;
                }
                .p_card{
                    border: 1px solid black;
                    width: 400px;
                    height: 600px;
                    @media (min-width: $tablet) and (max-width: $pc) {
                        width: 344px;
                        height: 516px;
                    }
                    @media (max-width: $mobile) {
                        width: 198px;
                        height: 328px;
                    }
                }
                .card{
                    .card_img{
                        padding: 140px 90px;
                        width: 220px;
                        img{width: 100%;}
                        @media (min-width: $tablet) and (max-width: $pc) {
                            width: 190px;
                            padding: 120px 77px;
                        }
                        @media (max-width: $mobile) {
                            width: 100px;
                            padding: 90px 50px
                        }
                    }
                    .card_contant{
                        padding: 50px 40px;
                        height: 400px;
                        @media (min-width: $tablet) and (max-width: $pc) {
                            padding: 40px 20px;
                            height: 350px;
                        }
                        @media (max-width: $mobile) {
                            padding: 20px 12px;
                            height: 240px;
                        }
                        h3{
                            font-size: 30px;
                            border-bottom: 2px solid black;
                            margin: 0;
                            padding-bottom: 30px;
                            margin-bottom: 50px;
                            cursor: default;
                            @media (min-width: $tablet) and (max-width: $pc) {
                                padding-bottom: 20px;
                                margin-bottom: 40px;
                            }
                            @media (max-width: $mobile) {
                                font-size: 19px;
                                padding-bottom: 10px;
                                margin-bottom: 10px;
                            }
                        }
                        p{
                            font-size: 25px;
                            cursor: default;
                            @media (max-width: $mobile) {
                                font-size: 18px;
                                margin: 10px 0;
                            }
                        }
                        .c_icon{
                            display: flex;
                            justify-content: center;
                            align-items: center;
                            gap: 30px;
                            @media (max-width: $mobile) {
                                gap: 20px;
                            }
                            .p_icon{
                                width: 60px;
                                cursor: pointer;
                                img{width: 100%;}
                                @media (min-width: $tablet) and (max-width: $pc) {
                                    width: 50px;
                                }
                                @media (max-width: $mobile) {
                                    width: 40px;
                                }
                            }
                            span{
                                border: 1px solid black;
                                height: 37px;
                                @media (min-width: $tablet) and (max-width: $pc)  {
                                    height: 30px;
                                }
                                @media (max-width: $mobile) {
                                    height: 24px;
                                }
                            }
                            .g_icon{
                                width: 55px;
                                cursor: pointer;
                                img{width: 100%;}
                                @media (min-width: $tablet) and (max-width: $pc) {
                                    width: 45px;
                                }
                                @media (max-width: $mobile) {
                                    width: 40px;
                                }
                            }
                        }
                    }
                }
                .card_name{
                    background-color: black;
                    color: white;
                    font-size: 30px;
                    padding: 30px 0;
                    margin: 0;
                    cursor: default;
                    @media (min-width: $tablet) and (max-width: $pc) {
                        padding: 25px 0;
                    }
                    @media (max-width: $mobile) {
                        font-size: 20px;
                        padding: 11px 0;
                    }
                }
            }
        }
        .about{
            margin: 300px 0;
            @media (max-width: $mobile) {
                margin: 0;
                height: 100vh;
            }
            .introduce{
                p{
                    margin: 100px 120px;
                    font-size: 1.875em; /* 30px */
                    @media (min-width: $pc) {
                        width: 820px;
                        padding: 0 180px;
                    }
                    @media (max-width: $mobile) {
                        margin: 80px 20px;
                        font-size: 20px;
                    }
                }
            }
            .key{
                display: flex;
                align-items: center;
                justify-content: space-around;
                .key_btn{
                    .btn.active{
                        opacity: 1;
                    }
                    .btn{
                        opacity: 0.2;
                        display: flex;
                        align-items: center;
                        justify-content: space-between;
                        gap: 20px;
                        margin: 20px;
                        cursor: pointer;
                        .S_img{
                            width: 30px;
                            img{width: 100%;}
                        }
                        span{
                            width: 100px;
                            text-align: left;
                            font-weight: 500;
                            font-size: 1.5em; /* 24px */
                            color: #333;
                        }
                    }
                }
                .key_word{
                    height: 600px;
                    .swiper{
                        height: 100%;
                    }
                    .swiper-slide{
                        opacity: 0.2;
                    }
                    .swiper-slide-active{
                        opacity: 1;
                    }
                    .word{
                        display: flex;
                        align-items: center;
                        justify-content: center;
                        gap: 50px;
                        .B_img{
                            width: 100px;
                            img{width: 100%;}
                        }
                        .kw{
                            width: 340px;
                            text-align: left;
                            cursor: default;
                            span{
                                color: #555;
                                font-weight: bold;
                                font-size: 1.5em; /* 24px */
                            }
                            p{
                                margin: 0;
                                font-size: 1.5em; /* 24px */
                            }
                        }
                    }
                }
                @media (max-width: $mobile) {
                    display: none;
                }
            }
            .tab{
                display: none;
                @media (max-width: $mobile) {
                    display: block;
                }
                .tab_button{
                    margin-bottom: 40px;
                    button.active{
                        opacity: 1;
                    }
                    button{
                        opacity: 0.2;
                        border: 0;
                        background-color: white;
                        width: 70px;
                        margin: 0 3px;
                        img{
                            width: 100%;
                        }
                    }
                }
                .tab_content{
                    .tab_text{
                        text-align: start;
                        margin: 0 20px;
                        word-break: keep-all;
                        dt{
                            font-size: 20px;
                            font-weight: 700;
                        }
                        dd{
                            margin: 0;
                            font-size: 20px;
                        }
                    }
                }
            }
        }
        .contact{
            height: 100vh;
            align-content: center;
            .conta{
                display: flex;
                align-items: center;
                justify-content: center;
                gap: 100px;
                @media (max-width: $mobile) {
                    flex-direction: column;
                    gap: 40px;
                }
                div{
                    display: inline-block;
                    cursor: pointer;
                    .conta_img{
                        width: 100px;
                        img{width: 100%;}
                        @media (max-width: $mobile) {
                            width: 60px;
                        }
                    }
                    h3{
                        font-size: 30px;
                        margin: 20px 0 0;
                        @media (max-width: $mobile) {
                            font-size: 20px;
                        }
                    }
                }
                span{
                    border: 1px solid black;
                    height: 150px;
                    @media (min-width: $tablet) and (max-width: $pc) {
                        height: 120px;
                    }
                    @media (max-width: $mobile) {
                        display: none;
                    }
                }
            }
        }
    }
</style>