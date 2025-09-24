"""
Конвертер площ (м², га, км²)
Консольне меню з підпунктами: Теорія, Конвертор, Формули, Автор, Вихід
Автор: Твоє ПІБ, група
"""

from collections import deque

# Константи: скільки м^2 в одиниці
UNIT_TO_M2 = {
    "m2": 1.0,
    "ha": 10_000.0,
    "km2": 1_000_000.0
}

UNIT_NAMES_UA = {
    "m2": "м²",
    "ha": "га",
    "km2": "км²"
}

# Історія конверсій (останні 10)
history = deque(maxlen=10)


def show_theory():
    print("\n=== Теорія ===")
    print("Одиниці площі:")
    print(" - м² (квадратний метр) — базова одиниця площі в СІ.")
    print(" - га (гектар) — 1 га = 10 000 м².")
    print(" - км² (квадратний кілометр) — 1 км² = 1 000 000 м².")
    print("\nФормула загально: value_B = value_A * (m2_in_A) / (m2_in_B)\n")
    input("Натисніть Enter щоб повернутися в меню...")


def show_formulas():
    print("\n=== Формули та пояснення ===")
    print("Перетворення через квадратний метр:")
    print(" - з м² в га: / 10 000")
    print(" - з м² в км²: / 1 000 000")
    print(" - з га в м²: * 10 000")
    print(" - з км² в м²: * 1 000 000")
    print("\nРекомендується округляти результат до 6 значущих або за потребою.")
    input("Натисніть Enter щоб повернутися в меню...")


def show_author():
    print("\n=== Інформація про автора ===")
    name = input("Введіть ваше ім'я та прізвище: ").strip()
    group = input("Введіть вашу групу: ").strip()
    email = input("Введіть ваш E-mail: ").strip()
    info = input("Коротко про себе/проєкт: ").strip()

    print("\n--- Автор ---")
    print(f"ПІБ: {name if name else 'Не вказано'}")
    print(f"Група: {group if group else 'Не вказано'}")
    print(f"E-mail: {email if email else 'Не вказано'}")
    print(f"Коротко: {info if info else 'Не вказано'}")

    input("\nНатисніть Enter щоб повернутися в меню...")
def choose_unit(prompt="Оберіть одиницю (введіть код):"):
    codes = list(UNIT_TO_M2.keys())
    while True:
        print(prompt)
        for i, code in enumerate(codes, start=1):
            print(f" {i}. {UNIT_NAMES_UA[code]} ({code})")
        choice = input("Ваш вибір (число або код, 'q' - скасувати): ").strip().lower()
        if choice == 'q':
            return None
        # дозволяємо вводити індекс або код
        if choice.isdigit():
            idx = int(choice) - 1
            if 0 <= idx < len(codes):
                return codes[idx]
        if choice in codes:
            return choice
        print("Невірний вибір. Спробуйте ще.")


def input_value(prompt="Введіть значення (число): "):
    while True:
        s = input(prompt).strip()
        if s.lower() in ('q', 'вихід', 'exit'):
            return None
        try:
            val = float(s)
            return val
        except ValueError:
            print("Невірне число. Введіть, будь ласка, дійсне число або 'q' щоб скасувати.")


def convert_area(value, unit_from, unit_to):
    # Формула через м^2
    m2 = value * UNIT_TO_M2[unit_from]
    result = m2 / UNIT_TO_M2[unit_to]
    return result


def show_history():
    print("\n=== Історія останніх конверсій ===")
    if not history:
        print("Історія пуста.")
    else:
        for i, rec in enumerate(history, start=1):
            print(f"{i}. {rec}")
    input("Натисніть Enter щоб повернутися в меню...")


def converter_menu():
    while True:
        print("\n--- Конвертор площ ---")
        print("1. Виконати конверсію")
        print("2. Історія конверсій")
        print("3. Повернутися в головне меню")
        choice = input("Вибір: ").strip()
        if choice == '1':
            val = input_value("Введіть значення (число, або 'q' щоб скасувати): ")
            if val is None:
                continue
            u_from = choose_unit("Оберіть одиницю, з якої конвертуємо:")
            if u_from is None:
                continue
            u_to = choose_unit("Оберіть одиницю, в яку конвертуємо:")
            if u_to is None:
                continue
            res = convert_area(val, u_from, u_to)
            # Округлення: 6 значущих, але якщо дуже велике/мале — використовувати формат
            formatted = f"{res:.6g}"
            print(f"\nРезультат: {val} {UNIT_NAMES_UA[u_from]} = {formatted} {UNIT_NAMES_UA[u_to]}")
            # Занести в історію
            history.appendleft(f"{val} {UNIT_NAMES_UA[u_from]} -> {formatted} {UNIT_NAMES_UA[u_to]}")
            input("Натисніть Enter щоб продовжити...")
        elif choice == '2':
            show_history()
        elif choice == '3':
            return
        else:
            print("Невірний вибір. Введіть 1, 2 або 3.")


def main_menu():
    while True:
        print("\n==============================")
        print("  Конвертер площ (м², га, км²)")
        print("==============================")
        print("1. Теорія")
        print("2. Конвертор площ")
        print("3. Формули та пояснення")
        print("4. Інформація про автора")
        print("5. Вихід")
        choice = input("Оберіть пункт меню (1-5): ").strip()
        if choice == '1':
            show_theory()
        elif choice == '2':
            converter_menu()
        elif choice == '3':
            show_formulas()
        elif choice == '4':
            show_author()
        elif choice == '5':
            print("До побачення!")
            break
        else:
            print("Невірний вибір. Спробуйте ще раз.")


if __name__ == "__main__":
    main_menu()
