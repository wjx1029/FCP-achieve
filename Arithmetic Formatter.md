def RemoveSpaceAndOperator(string):
    return string.translate(str.maketrans({' ':'','+':'','-':''}))

def RemoveSpace(string):
    return string.translate(str.maketrans({' ':''}))

def GetNumber(problems):
    numbers=[]
    for problem in problems:
        temp = []
        flag = True
        for _ in RemoveSpace(problem):
            if _ not in ['+','-'] and flag == True:
                temp.append(_)
            elif _ in ['+','-'] and flag == True:
                numbers.append(''.join(temp))
                flag = False
                temp = []
            else:
                temp.append(_)
        numbers.append(''.join(temp))
    return numbers

def GetOperator(problems):
    return ['+' if '+' in problem
            else '-'
            for problem in problems]

def GetFirstNumber(number):
    return number[::2]

def GetSecondNumber(number):
    return number[1::2]

def Calculate(problems):
    operators = GetOperator(problems)
    number_list = GetNumber(problems)
    result_list = []
    first_number = GetFirstNumber(number_list)
    second_number = GetSecondNumber(number_list)
    sign = [1 if operator == '+'
            else -1
            for operator in operators]

    for i in range(len(first_number)):
        result_list.append(int(first_number[i])+int(second_number[i])*(sign[i]))
    return result_list

def InfoProblem(problems):
    numbers = GetNumber(problems)
    first_numbers = GetFirstNumber(numbers)
    second_numbers = GetSecondNumber(numbers)

    info_list = []
    
    for i in range(int(len(numbers)/2)):
        first_number_length = len(first_numbers[i])
        second_number_length = len(second_numbers[i])
        number_length = max(first_number_length,second_number_length)
        problem_length = number_length + 2
        first_number = first_numbers[i]
        second_number = second_numbers[i]
        operator = GetOperator(problems)[i]
        cal_result = Calculate(problems)[i]
        cal_result_length = len(str(cal_result))
        #每个Problem的信息：0第一个数字的长度， 1第二个数字的长度， 2两个数字的最大长度， 3Problem所需的长度，4第一个数字， 5第二个数字， 6运算符号， 7计算结果， 8结果的长度
        problem_info = [first_number_length,second_number_length,number_length,problem_length,first_number,second_number,operator,cal_result,cal_result_length]
        info_list.append(problem_info)
    return info_list
#返回#每个Problem的信息：[i][0第一个数字的长度， 1第二个数字的长度， 2两个数字的最大长度， 3Problem所需的长度，4第一个数字， 5第二个数字， 6运算符号， 7计算结果，8结果的长度]

def DispSpecificLine(info_list,line):
    line =str(line)
    projectorsheet = {'1':[0,4],'2':[1,5],'3':[],'4':[8,7]}
    problem_amount = len(info_list)

    if line == '3':
        for i in range(problem_amount):
            print('-'*info_list[i][3] + (' '*4 if i != problem_amount-1 else ''),end=('' if i != problem_amount-1 else '\n'))
    elif line in ['1','2','4']:
        for i in range(problem_amount):
            for j in range(info_list[i][3]):
                if j == 0:
                    if line == '2':
                         print(info_list[i][6],end='')
                    else:
                        print(' ',end='')
                elif j != 0 and j < info_list[i][3] - info_list[i][projectorsheet[line][0]]:
                    print(' ',end='')
                else:
                    print(int(info_list[i][projectorsheet[line][1]]),end=('' if i != problem_amount-1 else '\n'))
                    break
            if i != problem_amount-1:
                print(' '*4,end='')
    else:
        raise ValueError('Line need less than or equal to 4.')
    
def DispFirstLine(info_list):
    DispSpecificLine(info_list,1)

def DispSecondLine(info_list):
    DispSpecificLine(info_list,2)

def DispThirdLine(info_list):
    DispSpecificLine(info_list,3)

def DispFourthLine(info_list):
    DispSpecificLine(info_list,4)

def arithmetic_arranger(problems, show_answers=False):
    if len(problems) > 5:
        raise ValueError('Error: Too many problems.')
    if sum([0 if '-' in problem or '+' in problem
            else 1
            for problem in problems]) != 0:
        raise TypeError("Error: Operator must be '+' or '-'.")
    if sum([0 if RemoveSpaceAndOperator(problem).isdigit()
            else 1
            for problem in problems]) != 0:
        raise ValueError('Error: Numbers must only contain digits.')
    if sum([0 if int(number) <= 9999 
            else 1
            for number in GetNumber(problems)]) != 0:
        raise ValueError('Error: Numbers cannot be more than four digits.')
    

    if show_answers == False:
        DispFirstLine(info_list=InfoProblem(problems))
        DispSecondLine(info_list=InfoProblem(problems))
        DispThirdLine(info_list=InfoProblem(problems))
    else:
        DispFirstLine(info_list=InfoProblem(problems))
        DispSecondLine(info_list=InfoProblem(problems))
        DispThirdLine(info_list=InfoProblem(problems))
        DispFourthLine(info_list=InfoProblem(problems))
    return problems
        
print(f'\n{arithmetic_arranger(["32 + 698", "3801 - 2", "45 + 43", "123 + 49"])}')

print(f'\n{arithmetic_arranger(["3801 - 2", "123 + 49"])}\n')

print(f'\n{arithmetic_arranger(["1 + 2", "1 - 9380"])}\n')

print(f'\n{arithmetic_arranger(["3 + 855", "3801 - 2", "45 + 43", "123 + 49"])}\n')

print(f'\n{arithmetic_arranger(["11 + 4", "3801 - 2999", "1 + 2", "123 + 49", "1 - 9380"])}\n')

#print(f'\n{arithmetic_arranger(["44 + 815", "909 - 2", "45 + 43", "123 + 49", "888 + 40", "653 + 87"])}\n')

#print(f'\n{arithmetic_arranger(["3 / 855", "3801 - 2", "45 + 43", "123 + 49"])}\n')

#print(f'\n{arithmetic_arranger(["24 + 85215", "3801 - 2", "45 + 43", "123 + 49"])}\n')

#print(f'\n{arithmetic_arranger(["98 + 3g5", "3801 - 2", "45 + 43", "123 + 49"])}\n')

print(f'\n{arithmetic_arranger(["3 + 855", "988 + 40"], True)}\n')

print(f'\n{arithmetic_arranger(["32 - 698", "1 - 3801", "45 + 43", "123 + 49", "988 + 40"], True)}\n')
