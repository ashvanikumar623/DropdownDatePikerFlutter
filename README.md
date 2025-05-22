Flutter Dropdown DatePicker


![dropdown_datepicker](https://github.com/user-attachments/assets/d9218ef8-7c90-4851-a213-a39dbaa48853)

//Custom function code -----------------------------

 Widget datePicker({
    required int selectedDay,
    required int selectedMonth,
    required int selectedYear,
    required int minYear,
    required int maxYear,
    required Function(int day, int month, int year) onDateChange,
  }) {
    const months = [
      'January',
      'February',
      'March',
      'April',
      'May',
      'June',
      'July',
      'August',
      'September',
      'October',
      'November',
      'December'
    ];

    return Row(
      mainAxisSize: MainAxisSize.max,
      children: [
        IconButton(
          onPressed: () {
            DateTime newDateTime =
                DateTime(selectedYear, selectedMonth, selectedDay)
                    .subtract(const Duration(days: 1));
            if (newDateTime.isAfter(DateTime(minYear - 1, 12, 31))) {
              onDateChange(
                  newDateTime.day, newDateTime.month, newDateTime.year);
            }
          },
          icon: const Icon(Icons.arrow_back_sharp),
        ),
        const SizedBox(width: 10),
        Expanded(
          child: Row(
            mainAxisSize: MainAxisSize.min,
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.center,
            children: [
              DropdownButtonHideUnderline(
                child: DropdownButton(
                  dropdownColor: AppColors.buttonSecondry,
                  value: selectedDay,
                  icon: const Icon(Icons.arrow_drop_down),
                  items: List.generate(
                          DateTime(selectedYear, selectedMonth + 1, 0).day,
                          (i) => i + 1)
                      .map((item) => DropdownMenuItem(
                            value: item,
                            child: Text(
                              item.toString(),
                              style: TextStyle(fontSize: 13),
                            ),
                          ))
                      .toList(),
                  onChanged: (value) {
                    onDateChange(value!, selectedMonth, selectedYear);
                  },
                ),
              ),
              const SizedBox(width: 10),
              DropdownButtonHideUnderline(
                child: DropdownButton(
                  dropdownColor: AppColors.buttonSecondry,
                  value: months[selectedMonth - 1],
                  icon: const Icon(Icons.arrow_drop_down),
                  items: months
                      .map((item) => DropdownMenuItem(
                            value: item,
                            child: Text(
                              item,
                              style: TextStyle(fontSize: 13),
                            ),
                          ))
                      .toList(),
                  onChanged: (value) {
                    if (selectedDay <=
                        DateTime(selectedYear, months.indexOf(value!) + 2, 0)
                            .day) {
                      onDateChange(
                          selectedDay, months.indexOf(value) + 1, selectedYear);
                    } else {
                      onDateChange(1, months.indexOf(value) + 1, selectedYear);
                    }
                  },
                ),
              ),
              const SizedBox(width: 10),
              DropdownButtonHideUnderline(
                child: DropdownButton(
                  dropdownColor: AppColors.buttonSecondry,
                  value: selectedYear,
                  icon: const Icon(Icons.arrow_drop_down),
                  items:
                      List.generate((maxYear - minYear) + 1, (i) => minYear + i)
                          .map((item) => DropdownMenuItem(
                                value: item,
                                child: Text(
                                  item.toString(),
                                  style: TextStyle(fontSize: 13),
                                ),
                              ))
                          .toList(),
                  onChanged: (value) {
                    if (selectedDay <=
                        DateTime(value!, selectedMonth + 1, 0).day) {
                      onDateChange(selectedDay, selectedMonth, value);
                    } else {
                      onDateChange(1, selectedMonth, value);
                    }
                  },
                ),
              ),
            ],
          ),
        ),
        const SizedBox(width: 10),
        IconButton(
          onPressed: () {
            DateTime newDateTime =
                DateTime(selectedYear, selectedMonth, selectedDay)
                    .add(const Duration(days: 1));
            if (newDateTime.isBefore(DateTime(maxYear + 1, 1, 1))) {
              onDateChange(
                  newDateTime.day, newDateTime.month, newDateTime.year);
            }
          },
          icon: const Icon(Icons.arrow_forward_sharp),
        ),
      ],
    );
  }


//Use like this -----------------------------

  datePicker(
    selectedDay: selectedDay,
    selectedMonth: selectedMonth,
    selectedYear: selectedYear,
    minYear: 2020,
    maxYear: 2050,
    onDateChange: (int day, int month, int year) {
      setState(() {
        selectedDay = day;
        selectedMonth = month;
        selectedYear = year;
      });
    },
  ),
