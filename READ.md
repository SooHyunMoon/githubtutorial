앱개발기초 기말프로젝트
Monthly와 Weekly 형식의 ToDoList
  
102반 2021661034 문수현

1. 과제 목적


앱 개발 분야의 실력을 키우기 위해 한 학기 동안 Android Studio 프로그램의 학습을 진행한 것을 바탕으로 하나의 완성된 앱을 만들어보며 개인의 실력을 키우는 것을 목표로 함.

2. 과제 추진 내용


- 수업 시간에 배운 RecyclerView와 ArrayList를 이용하여 Monthly와 Weekly가 합쳐진 하나의 ToDoList를 만들어 봄. 


[RecyclerView 사용 _ calendarRecyclerView]


    private RecyclerView calendarRecyclerView;
    
    private void intWidgets() {
        calendarRecyclerView = findViewById(R.id.calendarRecyclerView);
        monthYearText = findViewById(R.id.monthYear);
    }

    @RequiresApi(api = Build.VERSION_CODES.O)
    private void setMonthView() {
        monthYearText.setText(monthYearFromDate(CalendarUtils.selectedDate));
        ArrayList<LocalDate> daysInMonth = daysInMonthArray(CalendarUtils.selectedDate);

        CalendarAdapter calendarAdapter = new CalendarAdapter(daysInMonth, this);
        RecyclerView.LayoutManager layoutManager = new GridLayoutManager(getApplicationContext(), 7);
        calendarRecyclerView.setLayoutManager(layoutManager);
        calendarRecyclerView.setAdapter(calendarAdapter);
    }
    
    
[ArrayList 사용 _ daysInMonthArray, daysInWeekArray ]


    public static ArrayList<LocalDate> daysInMonthArray(LocalDate date) {
        ArrayList<LocalDate> daysInMonthArray = new ArrayList<>();
        YearMonth yearMonth = YearMonth.from(date);

        int daysInMonth = yearMonth.lengthOfMonth();

        LocalDate firstOfMonth = CalendarUtils.selectedDate.withDayOfMonth(1);
        int dayOfWeek = firstOfMonth.getDayOfWeek().getValue();

        for (int i = 1; i <= 42; i++) {
            if (i <= dayOfWeek || i > daysInMonth + dayOfWeek) {
                daysInMonthArray.add(null);
            } else {
                daysInMonthArray.add(LocalDate.of(selectedDate.getYear(), selectedDate.getMonth(),i - dayOfWeek));
            }
        }
        return daysInMonthArray;

    }

    @RequiresApi(api = Build.VERSION_CODES.O)
    public static ArrayList<LocalDate> daysInWeekArray(LocalDate selectedDate) {
        ArrayList<LocalDate> days = new ArrayList<>();
        LocalDate current = sundayForDate(selectedDate);
        LocalDate endDate = current.plusWeeks(1);

        while (current.isBefore(endDate)) {
            days.add(current);
            current = current.plusDays(1);
        }

        return days;
    }
  
- 시작 화면에 달과 년도 양 옆에 다음과 이전 버튼을 만들어 버튼을 누르면 다음 달 또는 이전 달로 넘어갈 수 있도록 작성함.

[previousMonthAction, nextMonthAction]


    public void previousMonthAction(View view) 
    {
        CalendarUtils.selectedDate = CalendarUtils.selectedDate.minusMonths(1);
        setMonthView();
    }
    public void nextMonthAction(View view) 
    {
        CalendarUtils.selectedDate = CalendarUtils.selectedDate.plusMonths(1);
        setMonthView();
    }
    
  
- Weekly 버튼을 누르면 한 주씩 나올 수 있도록 하였고, 밑에 New Event 버튼을 누르면 ToDoList를 추가할 수 있도록 작성함.

[weeklyAction, newEventAction]


    public void weeklyAction(View view) 
    {
        startActivity(new Intent(this, WeekViewActivity.class));
    }
    public void newEventAction(View view) 
    {
        startActivity(new Intent(this, EventEditActivity.class));
    }
  
  
- New Event 버튼을 누르면 내용을 작성하고 날짜와 시간은 자동으로 저장되도록 함.
