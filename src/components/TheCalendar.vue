<template>
    <div>
        <div class="flex flex-row">
            <div class="basis-1/2 w-1/2 cursor-pointer" @click="moveMonth('previous')">Back</div>
            <div class="basis-1/2 w-1/2 cursor-pointer" @click="moveMonth('next')">Forward</div>
        </div>
        <div>
            <span>{{monthDictionary[currentMonth].month}} {{currentYear}}</span>
        </div>
        <div id="calendar-container" class="border border-black">
            <div class="font-bold">Su</div>
            <div class="font-bold">Mo</div>
            <div class="font-bold">Tu</div>
            <div class="font-bold">We</div>
            <div class="font-bold">Th</div>
            <div class="font-bold">Fr</div>
            <div class="font-bold">Sa</div>
            <div class="border border-gray" style="min-height: 150px" v-for="day in daysArray" :key="day.id">{{ day.dayOfMonth }}</div>
        </div>
    </div>

</template>

<script>
    export default {
        data() {
            return {
                currentMonth: new Date().getMonth(),
                currentYear: new Date().getFullYear(),
                daysArray: [],
                monthDictionary: {
                    0: {
                        month: 'January',
                        appr: 'Jan'
                    },
                    1: {
                        month: 'February',
                        appr: 'Feb'
                    },
                    2: {
                        month: 'March',
                        appr: 'Mar'
                    },
                    3: {
                        month: 'April',
                        appr: 'Apr'
                    },
                    4: {
                        month: 'May',
                        appr: 'May'
                    },
                    5: {
                        month: 'June',
                        appr: 'Jun'
                    },
                    6: {
                        month: 'July',
                        appr: 'Jul'
                    },
                    7: {
                        month: 'August',
                        appr: 'Aug'
                    },
                    8: {
                        month: 'September',
                        appr: 'Sep'
                    },
                    9: {
                        month: 'October',
                        appr: 'Oct'
                    },
                    10: {
                        month: 'November',
                        appr: 'Nov'
                    },
                    11: {
                        month: 'December',
                        appr: 'Dec'
                    }
                }
            }
        },
        methods: {
            setMonthData(currentMonth, currentYear) {
                function getNumberOfDays (_month, _year) {
                    if (_month === -1) _year = _year -1 ;
                    return 32 - new Date (_year, _month, 32).getDate();
                }
                function setDateObj(passedDay, month, year, outOfMonth) {
                    const day = new Date(year, month, passedDay);
                    return {
                        id: day.getTime(),
                        year: year,
                        month: month,
                        dayOfMonth: passedDay,
                        previousMount: outOfMonth
                        // dayOfWeek: null,
                    }
                }
                function fillPreviousDays(fistDay, lastDay, month, year) {
                    if (firstDay === 0) return false;
                    if (month === -1) {
                        year = year - 1 ;
                        month = 11
                    }
                    const fillDays = [];
                    let incrementer = fistDay;
                    while (incrementer > 0) {
                        let dateObj = setDateObj(lastDay, month, year, true);
                        fillDays.unshift(dateObj);
                        lastDay--;
                        incrementer--;
                    }
                    return fillDays
                }
                function generateCalendarArray(days, prevArray, month, year) {
                    let fillCalendar = []
                    for (let i = 1; i <= days; i++) {
                        let dateObj = setDateObj(i, month, year, false);
                        fillCalendar.push(dateObj)
                    }
                    if (prevArray) {
                        fillCalendar = prevArray.concat(fillCalendar)
                    }
                    if (fillCalendar.length < 42) {
                        let count = 1;
                        let nextMonth = month + 1
                        if (nextMonth === 12) {
                            nextMonth = 0;
                            year = year + 1;
                        }
                        while (fillCalendar.length < 42) {
                            let dateObj = setDateObj(count, nextMonth, year, true);
                            fillCalendar.push(dateObj);
                            count++
                        }
                    }
                    return fillCalendar
                }
                const firstDay = new Date(currentYear, currentMonth).getDay()
                const daysInMonth = getNumberOfDays(currentMonth, currentYear)
                const daysInPrevMonth = getNumberOfDays(currentMonth - 1, currentYear)
                const daysOfPastGoneBy = fillPreviousDays(firstDay, daysInPrevMonth, currentMonth - 1, currentYear)
                this.daysArray = generateCalendarArray(daysInMonth, daysOfPastGoneBy, currentMonth, currentYear)
                console.log('daysArray ', this.daysArray);
            },
            moveMonth(direction){
                if (direction === 'next') {
                    this.currentMonth = this.currentMonth + 1;
                    if (this.currentMonth === 12) {
                        this.currentMonth = 0;
                        this.currentYear = this.currentYear + 1;
                    }
                    this.setMonthData(this.currentMonth, this.currentYear);
                }
                if (direction === 'previous') {
                    this.currentMonth = this.currentMonth - 1;
                    if (this.currentMonth === -1) {
                        this.currentMonth = 11;
                        this.currentYear = this.currentYear - 1;
                    }
                    this.setMonthData(this.currentMonth, this.currentYear);
                }
            },
        },
        beforeMount() {
            this.setMonthData(this.currentMonth, this.currentYear);
        }
    }
</script>

<style scoped>
    #calendar-container {
        display: grid;
        grid-template-columns: repeat(7, 1fr);
        grid-template-rows: 0.3fr repeat(6, 1fr);
        grid-column-gap: 0px;
        grid-row-gap: 0px;
    }
</style>