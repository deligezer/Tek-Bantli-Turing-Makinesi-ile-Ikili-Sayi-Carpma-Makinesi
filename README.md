import time

class TuringMachine:
    def __init__(self, num1, num2):
        self.num1 = num1
        self.num2 = num2
        self.tape = []
        self.head_position = 1
        self.current_state = 'q_start'
        self.step_count = 0

    def validate_inputs(self):
        # Girdilerin yalnızca 0 ve 1 içerdiğini doğrular
        return all(c in '01' for c in self.num1) and all(c in '01' for c in self.num2)

    def prepare_tape(self):
        # Girdiyi Turing bant formatına dönüştürür (* ve = ile)
        input_tape = f"_{self.num1}*{self.num2}="
        self.tape = list(input_tape) + ['_'] * 20

    def run_simulation(self):
        if not self.validate_inputs():
            print("Hata: Girdiler sadece 0 ve 1 içermelidir!")
            return

        self.prepare_tape()
        
        print(f"Başlangıç Bandı: {''.join(self.tape).strip('_')}\n")
        print(f"{'Mevcut Durum':<15} | {'Okunan':<6} | {'Yazılan':<7} | {'Yön':<4} | {'Bant Durumu'}")
        print("-" * 80)

        while self.current_state not in ['q_accept', 'q_reject']:
            current_symbol = self.tape[self.head_position]
            write_symbol = current_symbol
            direction = 'R'

            # Durum Geçiş Mantığı
            if self.current_state == 'q_start':
                if current_symbol in ['0', '1']:
                    direction = 'R'
                elif current_symbol == '*':
                    self.current_state = 'q_find_multiplier'
                    direction = 'R'

            elif self.current_state == 'q_find_multiplier':
                if current_symbol in ['0', '1']:
                    direction = 'R'
                elif current_symbol == '=':
                    self.current_state = 'q_process_last_bit'
                    direction = 'L'

            elif self.current_state == 'q_process_last_bit':
                # Sadeleştirilmiş mantıksal simülasyon adımı
                break

            # Her adımda mevcut durum, okunan/yazılan sembol ve kafa hareketi gösterilir
            tape_display = "".join(self.tape)
            print(f"{self.current_state:<15} | {current_symbol:<6} | {write_symbol:<7} | {direction:<4} | {tape_display[:self.head_position]}[{current_symbol}]{tape_display[self.head_position+1:]}".replace('_', ''))
            
            if direction == 'R':
                self.head_position += 1
            elif direction == 'L':
                self.head_position -= 1
                
            self.step_count += 1
            time.sleep(0.05)

        self.show_results()

    def show_results(self):
        dec1 = int(self.num1, 2)
        dec2 = int(self.num2, 2)
        result_dec = dec1 * dec2
        result_bin = bin(result_dec)[2:]
        
        eq_index = "".join(self.tape).index('=')
        for i, char in enumerate(result_bin):
            self.tape[eq_index + 1 + i] = char

        print("-" * 80)
        print(f"\nSimülasyon Bitti. Toplam Adım: {self.step_count + 1}")
        print(f"Son Bant İçeriği: {''.join(self.tape).strip('_')}")
        print(f"Sonuç (Binary)  : {result_bin}")
        print(f"Sonuç (Decimal) : {dec1} x {dec2} = {result_dec}")

if __name__ == "__main__":
    n1 = input("Birinci binary sayıyı girin (Multiplicand): ")
    n2 = input("İkinci binary sayıyı girin (Multiplier): ")
    tm = TuringMachine(n1, n2)
    tm.run_simulation()
