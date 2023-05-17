<template>
    <Page>
        <ActionBar>
            <Label text="Calendar" />
        </ActionBar>
        <GridLayout rows="auto,*,auto"
            columns="*">
            <FlexboxLayout col="0"
                row="0"
                flexDirection="column">
                <FlexboxLayout>
                    <label class="today"
                        text="Сегодня " />
                    <label class="today"
                        :text="this.currentDay" />
                    <label class="today"
                        text=" " />
                    <label class="today"
                        :text="this.months[this.currentMonth - 1].form" />
                    <label class="today"
                        text=" " />
                    <label class="today"
                        :text="this.currentYear" />
                    <label class="today"
                        text=" г." />
                </FlexboxLayout>
                <FlexboxLayout class="cur">
                    <label class="current"
                        :text="this.months[this.month - 1].name" />
                    <label class="current"
                        text=" " />
                    <label class="current"
                        :text="this.year" />
                    <label class="current"
                        text=" г." />
                </FlexboxLayout>
            </FlexboxLayout>
            <DockLayout row="1" col="0" class="cal">
                <GridLayout dock="top"
                    columns="*,*,*,*,*,*,*"
                    rows="*"
                    class="weekdays">
                    <label text="Пн"
                        col="0"
                        row="0" />
                    <label text="Вт"
                        col="1"
                        row="0" />
                    <label text="Ср"
                        col="2"
                        row="0" />
                    <label text="Чт"
                        col="3"
                        row="0" />
                    <label text="Пт"
                        col="4"
                        row="0" />
                    <label text="Сб"
                        col="5"
                        row="0" />
                    <label text="Вс"
                        col="6"
                        row="0" />
                </GridLayout>
                <WrapLayout dock="bottom">
                    <template v-for="index in emptyDays">
                        <label text=""
                            width="14%" />
                    </template>
                    <template v-for="index in monthDays">
                        <template v-if="index == currentDay && month == currentMonth && year == currentYear">
                            <label :text="index"
                                width="14%"
                                :style="{ 'background-color': dayNowColor }"
                                class="days" />
                        </template>
                        <template v-else>
                            <label :text="index"
                                width="14%"
                                class="days"
                                :style="{ 'background-color': standardColor }" />
                        </template>
                    </template>
                </WrapLayout>
            </DockLayout>
            <DockLayout col="0"
                row="2"
                class="selector-month"
                stretchLastChild="false">
                <FlexboxLayout dock="bottom">
                    <Button text="<"
                        @tap="previous"
                        width="28%" />
                    <Button text="home"
                        @tap="home"
                        width="28%" />
                    <Button text=">"
                        @tap="next"
                        width="28%" />
                </FlexboxLayout>
            </DockLayout>
        </GridLayout>
    </Page>
</template>

<script>

export default {
    data() {
        return {
            dayNowColor: "#FF0000",
            standardColor: "#2e2e2e",
            year: 0,
            month: 1,
            day: 0,
            currentYear: 0,
            currentMonth: 1,
            currentDay: 0,
            monthDays: 0,
            date: "",
            firstWeekday: "",
            emptyDays: 0,
            months: [
                {
                    name: "Январь",
                    form: "Января",
                    code: 1
                },
                {
                    name: "Февраль",
                    form: "Февраля",
                    code: 4
                },
                {
                    name: "Март",
                    form: "Марта",
                    code: 4
                },
                {
                    name: "Апрель",
                    form: "Апреля",
                    code: 0
                },
                {
                    name: "Май",
                    form: "Мая",
                    code: 2
                },
                {
                    name: "Июнь",
                    form: "Июня",
                    code: 5
                },
                {
                    name: "Июль",
                    form: "Июля",
                    code: 0
                },
                {
                    name: "Август",
                    form: "Августа",
                    code: 3
                },
                {
                    name: "Сентябрь",
                    form: "Сентября",
                    code: 6
                },
                {
                    name: "Октябрь",
                    form: "Октября",
                    code: 1
                },
                {
                    name: "Ноябрь",
                    form: "Ноября",
                    code: 4
                },
                {
                    name: "Декабрь",
                    form: "Декабря",
                    code: 6
                },
            ]
        };
    },
    methods: {
        getDaysInMonth(year, month) {
            return new Date(year, month, 0).getDate();
        },
        getYearCode(year) {
            let code = (6 + Number(year.toString().slice(-2)) + Math.trunc(Number(year.toString().slice(-2)) / 4)) % 7;
            return code;
        },
        firstWeekdayInMonth(year, month, day) {
            let firstWeekday = (1 + this.months[month - 1].code + this.getYearCode(year)) % 7 - this.isVisokosniy(year, month);
            return firstWeekday;
        },
        isVisokosniy(year, month) {
            if ((year % 4 == 0) && (month == 1 || month == 2)) {
                return 1;
            }
            ;
            return 0;
        },
        getEmptyDays(firstWeekday) {
            let emptyDays = (firstWeekday - 2 + 7) % 7;
            return emptyDays;
        },
        renderCurrentCalendar() {
            this.date = new Date();
            this.currentYear = this.date.getFullYear();
            this.currentMonth = this.date.getMonth() + 1;
            this.currentDay = this.date.getDate();
            this.year = this.currentYear;
            this.month = this.currentMonth;
            this.day = this.currentDay;
            this.monthDays = this.getDaysInMonth(this.currentYear, this.currentMonth);
            this.firstWeekday = this.firstWeekdayInMonth(this.currentYear, this.currentMonth, this.currentDay);
            this.emptyDays = this.getEmptyDays(this.firstWeekday);
        },
        renderCalendar() {
            this.monthDays = this.getDaysInMonth(this.year, this.month);
            this.firstWeekday = this.firstWeekdayInMonth(this.year, this.month, this.day);
            this.emptyDays = this.getEmptyDays(this.firstWeekday);
        },
        next() {
            this.month++;
            if (this.month == 13) {
                this.month = 1;
                this.year++;
            }
            this.renderCalendar();
        },
        previous() {
            this.month--;
            if (this.month == 0) {
                this.month = 12;
                this.year--;
            }
            this.renderCalendar();
        },
        home() {
            this.renderCurrentCalendar();
        }
    },
    mounted() {
        this.renderCurrentCalendar();
    },
};
</script>

<style scoped lang="scss">
@import '@nativescript/theme/scss/variables/blue';

// Custom styles
.fas {
    @include colorize($color: accent);
}

.info {
    font-size: 20;
    horizontal-align: center;
    vertical-align: center;
}
</style>
