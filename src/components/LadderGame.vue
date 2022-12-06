<template>
    <v-container>
        <v-card-title class="text-center justify-center home_banner home_banner_text">
            팀 정하기
        </v-card-title>
        <v-row no-gutters>
            <v-col cols="2"></v-col>
            <v-col cols="4">
                <v-card-text class="home_banner_text">팀 개수</v-card-text>
                <ButtonCounter @team="val => this.teamLength = val"/>
            </v-col>
            <v-col cols="4" align-self="center" class="d-flex justify-center">
                <v-btn :color="'#95adc4'" @click="setTeam">팀 나누기</v-btn>
                <v-btn :color="'orange'" @click="reset" class="ml-3">선택 초기화</v-btn>
            </v-col>
            <v-cols cols="2"></v-cols>
        </v-row>
        <v-spacer style="width:100%; height: 80px"/>
        <v-tabs v-model="tab" color="red">
            <v-tab v-for="item in tabs" :key="item">{{ item }}</v-tab>
        </v-tabs>
        <v-tabs-slider></v-tabs-slider>
        <v-tabs-items v-model="tab" class="tabs_item">
            <v-tab-item eager :key="1">
                <LineChoicer key="1" :list="defaultList" :team-count="teamLength" :handleBtn="firstTabFlag" :text="texts[0]" :reset-btn="resetFlag"></LineChoicer>
            </v-tab-item>
            <v-tab-item eager :key="2">
                <LineChoicer key="t" :list="defaultTP" :team-count="teamLength" :handleBtn="secondTabFlag" :text="texts[1]" :reset-btn="resetFlag"></LineChoicer>
                <LineChoicer key="j" :list="defaultJG" :team-count="teamLength" :handleBtn="secondTabFlag" :text="texts[2]" :reset-btn="resetFlag"></LineChoicer>
                <LineChoicer key="m" :list="defaultMD" :team-count="teamLength" :handleBtn="secondTabFlag" :text="texts[3]" :reset-btn="resetFlag"></LineChoicer>
                <LineChoicer key="a" :list="defaultAD" :team-count="teamLength" :handleBtn="secondTabFlag" :text="texts[4]" :reset-btn="resetFlag"></LineChoicer>
                <LineChoicer key="s" :list="defaultSP" :team-count="teamLength" :handleBtn="secondTabFlag" :text="texts[5]" :reset-btn="resetFlag"></LineChoicer>
            </v-tab-item>
        </v-tabs-items>
    </v-container>
</template>

<script>
import ButtonCounter from "./ButtonCounter";
import LineChoicer from "@/components/LineChoicer";

export default {
    name: "LadderGame",
    data: () => ({
        tab: 0,
        firstTabFlag: false,
        secondTabFlag: false,
        resetFlag: false,
        tabs: ['무작위', '라인별'],
        texts: ['팀원 추가', '탑', '정글', '미드', '원딜', '서폿'],
        defaultList: ['프로파오후', 'Poltn', '샤쓰', 'Kaco', 'DHA', '댄디댄디', '럿트', '바둠랑이', '욕먹기대회우승자'],
        defaultTP:['플라나리아','바둠랑이'],
        defaultJG:['댄디댄디','샤쓰'],
        defaultMD:['Kaco','럿트'],
        defaultAD:['Poltn','욕먹기대회우승자'],
        defaultSP:['프로파오후','DHA'],
        teamLength: 2,
    }),
    components: {
        LineChoicer,
        ButtonCounter,
    },
    methods: {
        setTeamRandomMode(){
            this.firstTabFlag = !this.firstTabFlag
        },
        setTeamLinerMode() {
            this.secondTabFlag = !this.secondTabFlag
        },
        setTeam() {
            // random mode
            if (this.tab === 0)
                this.setTeamRandomMode();
            // liner mode
            else if (this.tab === 1)
                this.setTeamLinerMode();
        },
        reset(){
            this.resetFlag = !this.resetFlag
        }
    },
}
</script>

<style scoped>
.home_banner_text {
    width: 100%;
    text-align: center;
    vertical-align: center;
    margin: 0;
}

.home_banner {
    height: 200px;
    line-height: 200px;
    font-size: 1.4rem;
}

.home_chip_group {
    max-width: 80%;
    width: fit-content;
    margin: 0 auto;
}

.home_add_team {
    width: 100px;
}

.tabs_item {
    margin: 20px 0;
}
</style>