# bmi calculator

 

def is_price_range_valid(price_lower_limit, price_upper_limit):
    result = True

    if float(price_lower_limit) > float(price_upper_limit):
        result = False
    elif float(price_upper_limit) < 0 or float(price_lower_limit) < 0:
        result = False
    else:
        result = True

    return result

def calc_average_expenses(csv_list):
    total = 0
    average = 0
    for item in csv_list:
        total = total + float(item['price'])
    
    average = total / len(csv_list)
    return average

def calc_total_expenses(csv_list):
    total_expenses = 0
    for item in csv_list:
        total_expenses = total_expenses + float(item['price'])
    return total_expenses

def get_items_by_price_range(csv_list, str_price_lower_limit, str_price_upper_limit):
    result = []

    for item in csv_list:
        item_price = item['price']
        if float(item_price) >= float(str_price_lower_limit) and float(str_price_upper_limit) >= float(item_price):
            result.append(item)
            
    return result

def sort_by_items(csv_list):
    result = []
    final = []

    for item in csv_list:
        result.append(item['expense_item'])
    result_sorted = sorted(result)
    for name in result_sorted:
        for item in csv_list:
            if item['expense_item'] == name:
                final.append(item)

    return final

def display_main_menu():
    print("\n----- Expense Tracker -----")
    print("Select option\n")
    print("1 - Display all records")
    print("2 - Display statistics")
    print("3 - Display items within price range")
    print("Q - Quit")
    option = input("Enter selection =>")
    csv_list = load_csv_database()

    if option == '1':
        display_all_records(csv_list)
    elif option == '2':
        get_expenses_statistics(csv_list)
    elif option == '3':
        price_lower_limit = input("Price (Lower Limit) = ")
        price_upper_limit = input("Price (Upper Limit) = ")

        if is_price_range_valid(price_lower_limit, price_upper_limit):
            result_list = get_items_by_price_range(csv_list, price_lower_limit, price_upper_limit)
            display_results(result_list)
        else:
            print("\nInvalid Price Range entered!")
    elif option == '4':
        result_list = sort_by_items(csv_list)
        display_results(result_list)
    elif option == 'Q':
        print("Thank You and have a nice day :-)")
        quit()

def load_csv_database():
    print("\nLoading CSV file database")

    with open('expenses.csv', mode='r') as csv_file:
        csv_dict = csv.DictReader(csv_file)
        csv_list = list(csv_dict)
        return csv_list

def display_results(result_list):
    for item in result_list:
        print(item["date"] + "\t\t" + item["expense_item"] + "\t\t" + item["price"])

def display_all_records(csv_list):
    print("Date\t\t\tExpense Item\t\tPrice")
    for item in csv_list:
        print(item["date"] + "\t\t"+ item["expense_item"] + "\t\t\t\t" + item["price"])

def get_expenses_statistics(csv_list):
    print("\nExpenses Statistics")
    print("-------------------")
    print("Total Expenses = $" + str(calc_total_expenses(csv_list)))
    print("Average Expenses = $" + str(calc_average_expenses(csv_list)))

def main():
    while (True):
        display_main_menu()

if __name__ == "__main__":
    main()


def test_calc_average_expenses():
    csv_list = labtest1.load_csv_database()
    expected = 6.0
    result = labtest1.calc_average_expenses(csv_list)

    assert result == expected

def test_calc_total_expenses():
    csv_list = labtest1.load_csv_database()
    expected = 42.0
    result = labtest1.calc_total_expenses(csv_list)
    assert (result == expected)

def test_is_price_range_valid():
    upper1 = 5
    lower1 = 3

    upper2 = -1
    lower2 = -4

    result1 = labtest1.is_price_range_valid(upper1, lower1)
    result2 = labtest1.is_price_range_valid(upper2, lower2)

    assert result1 == result2 == 0

def test_is_price_range_valid():
    upper1 = "5"
    lower1 = "3"

    upper2 = "-1"
    lower2 = "-4"

    result1 = labtest1.is_price_range_valid(lower1, upper1)
    result2 = labtest1.is_price_range_valid(lower2, upper2)

    assert result1 == True
    assert result2 == False

def test_get_items_by_price_range():
    csv_list = labtest1.load_csv_database()
    lower = 3.5
    upper = 8

    expected = [
        {"date": "15.01.2022", "expense_item": "butter", "price": "3.5"},
        {"date": "19.01.2022", "expense_item": "apples", "price": "4.7"}
    ]

    result = labtest1.get_items_by_price_range(csv_list, lower, upper)

    assert result == expected

def test_get_items_by_price_range():
    csv_list = labtest1.load_csv_database()
    lower = "3.5"
    upper = "8"

    expected = [
        {"date": "15.01.2022", "expense_item": "butter", "price": "3.5"},
        {"date": "19.01.2022", "expense_item": "apples", "price": "4.7"}
    ]

    result = labtest1.get_items_by_price_range(csv_list, lower, upper)

    assert result == expected

def test_sort_by_items():
    csv_list = labtest1.load_csv_database()

def test_sort_by_items():
    csv_list = labtest1.load_csv_database()

    expected = [
        {"date": "19.01.2022", "expense_item": "apples", "price": "4.7"},
        {"date": "15.01.2022", "expense_item": "butter", "price": "3.5"},
        {"date": "12.01.2022", "expense_item": "milk", "price": "7.5"}
    ]

    result = labtest1.sort_by_items(csv_list)

    assert result == expected




