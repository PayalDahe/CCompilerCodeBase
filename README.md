import re

# Token types
TOKEN_NUMBER = "NUMBER"
TOKEN_PLUS = "PLUS"
TOKEN_MINUS = "MINUS"
TOKEN_MULTIPLY = "MULTIPLY"
TOKEN_DIVIDE = "DIVIDE"
TOKEN_LPAREN = "LPAREN"
TOKEN_RPAREN = "RPAREN"
TOKEN_END = "END"

# Token class
class Token:
    def __init__(self, type_, value=None):
        self.type = type_
        self.value = value

    def __repr__(self):
        return f"Token({self.type}, {self.value})"

# Tokenizer function
def tokenize(input_string):
    token_specification = [
        (TOKEN_NUMBER, r'\d+'),        # Integer numbers
        (TOKEN_PLUS, r'\+'),          # Plus
        (TOKEN_MINUS, r'-'),          # Minus
        (TOKEN_MULTIPLY, r'\*'),      # Multiply
        (TOKEN_DIVIDE, r'/'),         # Divide
        (TOKEN_LPAREN, r'\('),        # Left parenthesis
        (TOKEN_RPAREN, r'\)'),        # Right parenthesis
        ('SKIP', r'[ \t]+'),          # Skip spaces and tabs
    ]
    token_regex = '|'.join(f'(?P<{pair[0]}>{pair[1]})' for pair in token_specification)
    tokens = []
    for match in re.finditer(token_regex, input_string):
        type_ = match.lastgroup
        value = match.group(type_)
        if type_ == 'SKIP':
            continue
        elif type_ == TOKEN_NUMBER:
            value = int(value)  # Convert numbers to integers
        tokens.append(Token(type_, value))
    tokens.append(Token(TOKEN_END))  # Add an end token
    return tokens
# Parser class
class Parser:
    def __init__(self, tokens):
        self.tokens = tokens
        self.pos = 0

    def current_token(self):
        return self.tokens[self.pos]

    def eat(self, token_type):
        if self.current_token().type == token_type:
            self.pos += 1
        else:
            raise SyntaxError(f"Expected {token_type}, got {self.current_token().type}")

    def parse_factor(self):
        token = self.current_token()
        if token.type == TOKEN_NUMBER:
            self.eat(TOKEN_NUMBER)
            return token.value
        elif token.type == TOKEN_LPAREN:
            self.eat(TOKEN_LPAREN)
            result = self.parse_expression()
            self.eat(TOKEN_RPAREN)
            return result
        else:
            raise SyntaxError("Invalid factor")

    def parse_term(self):
        result = self.parse_factor()
        while self.current_token().type in (TOKEN_MULTIPLY, TOKEN_DIVIDE):
            token = self.current_token()
            if token.type == TOKEN_MULTIPLY:
                self.eat(TOKEN_MULTIPLY)
                result *= self.parse_factor()
            elif token.type == TOKEN_DIVIDE:
                self.eat(TOKEN_DIVIDE)
                result //= self.parse_factor()  # Integer division
        return result

    def parse_expression(self):
        result = self.parse_term()
        while self.current_token().type in (TOKEN_PLUS, TOKEN_MINUS):
            token = self.current_token()
            if token.type == TOKEN_PLUS:
                self.eat(TOKEN_PLUS)
                result += self.parse_term()
            elif token.type == TOKEN_MINUS:
                self.eat(TOKEN_MINUS)
                result -= self.parse_term()
        return result
def main():
    while True:
        try:
            input_string = input("Enter expression: ")
            if input_string.lower() in ("exit", "quit"):
                break

            # Tokenize the input
            tokens = tokenize(input_string)
            print("Tokens:", tokens)

            # Parse and evaluate the expression
            parser = Parser(tokens)
            result = parser.parse_expression()
            print("Result:", result)

        except Exception as e:
            print("Error:", e)
# Update token types
TOKEN_IF = "IF"
TOKEN_ELSE = "ELSE"
TOKEN_EQ = "EQ"      # ==
TOKEN_NEQ = "NEQ"    # !=
TOKEN_LT = "LT"      # <
TOKEN_GT = "GT"      # >
TOKEN_LTE = "LTE"    # <=
TOKEN_GTE = "GTE"    # >=
TOKEN_SEMICOLON = "SEMICOLON"  # ;

# Update tokenizer
def tokenize(input_string):
    token_specification = [
        (TOKEN_IF, r'if'),               # if keyword
        (TOKEN_ELSE, r'else'),           # else keyword
        (TOKEN_EQ, r'=='),               # ==
        (TOKEN_NEQ, r'!='),              # !=
        (TOKEN_LTE, r'<='),              # <=
        (TOKEN_GTE, r'>='),              # >=
        (TOKEN_LT, r'<'),                # <
        (TOKEN_GT, r'>'),                # >
        (TOKEN_NUMBER, r'\d+'),          # Numbers
        (TOKEN_PLUS, r'\+'),             # +
        (TOKEN_MINUS, r'-'),             # -
        (TOKEN_MULTIPLY, r'\*'),         # *
        (TOKEN_DIVIDE, r'/'),            # /
        (TOKEN_LPAREN, r'\('),           # (
        (TOKEN_RPAREN, r'\)'),           # )
        (TOKEN_SEMICOLON, r';'),         # ;
        ('SKIP', r'[ \t]+'),             # Skip spaces
    ]
    token_regex = '|'.join(f'(?P<{pair[0]}>{pair[1]})' for pair in token_specification)
    tokens = []
    for match in re.finditer(token_regex, input_string):
        type_ = match.lastgroup
        value = match.group(type_)
        if type_ == 'SKIP':
            continue
        elif type_ == TOKEN_NUMBER:
            value = int(value)  # Convert numbers to integers
        tokens.append(Token(type_, value))
    tokens.append(Token(TOKEN_END))  # Add end token
    return tokens
class Parser:
    def __init__(self, tokens):
        self.tokens = tokens
        self.pos = 0

    def current_token(self):
        return self.tokens[self.pos]

    def eat(self, token_type):
        if self.current_token().type == token_type:
            self.pos += 1
        else:
            raise SyntaxError(f"Expected {token_type}, got {self.current_token().type}")

    def parse_factor(self):
        token = self.current_token()
        if token.type == TOKEN_NUMBER:
            self.eat(TOKEN_NUMBER)
            return token.value
        elif token.type == TOKEN_LPAREN:
            self.eat(TOKEN_LPAREN)
            result = self.parse_expression()
            self.eat(TOKEN_RPAREN)
            return result
        else:
            raise SyntaxError("Invalid factor")

    def parse_term(self):
        result = self.parse_factor()
        while self.current_token().type in (TOKEN_MULTIPLY, TOKEN_DIVIDE):
            token = self.current_token()
            if token.type == TOKEN_MULTIPLY:
                self.eat(TOKEN_MULTIPLY)
                result *= self.parse_factor()
            elif token.type == TOKEN_DIVIDE:
                self.eat(TOKEN_DIVIDE)
                result //= self.parse_factor()  # Integer division
        return result

    def parse_expression(self):
        result = self.parse_term()
        while self.current_token().type in (TOKEN_PLUS, TOKEN_MINUS):
            token = self.current_token()
            if token.type == TOKEN_PLUS:
                self.eat(TOKEN_PLUS)
                result += self.parse_term()
            elif token.type == TOKEN_MINUS:
                self.eat(TOKEN_MINUS)
                result -= self.parse_term()
        return result

    def parse_condition(self):
        left = self.parse_expression()
        token = self.current_token()
        if token.type in (TOKEN_EQ, TOKEN_NEQ, TOKEN_LT, TOKEN_GT, TOKEN_LTE, TOKEN_GTE):
            self.eat(token.type)
            right = self.parse_expression()
            if token.type == TOKEN_EQ:
                return left == right
            elif token.type == TOKEN_NEQ:
                return left != right
            elif token.type == TOKEN_LT:
                return left < right
            elif token.type == TOKEN_GT:
                return left > right
            elif token.type == TOKEN_LTE:
                return left <= right
            elif token.type == TOKEN_GTE:
                return left >= right
        else:
            raise SyntaxError("Invalid condition")

    def parse_if_else(self):
        self.eat(TOKEN_IF)
        self.eat(TOKEN_LPAREN)
        condition = self.parse_condition()
        self.eat(TOKEN_RPAREN)
        if condition:
            result = self.parse_statement()
        else:
            if self.current_token().type == TOKEN_ELSE:
                self.eat(TOKEN_ELSE)
                result = self.parse_statement()
            else:
                result = None
        return result

    def parse_statement(self):
        if self.current_token().type == TOKEN_IF:
            return self.parse_if_else()
        else:
            result = self.parse_expression()
            self.eat(TOKEN_SEMICOLON)
            return result
def main():
    while True:
        try:
            input_string = input("Enter statement: ")
            if input_string.lower() in ("exit", "quit"):
                break

            # Tokenize the input
            tokens = tokenize(input_string)
            print("Tokens:", tokens)

            # Parse and execute the statement
            parser = Parser(tokens)
            result = parser.parse_statement()
            print("Result:", result)

        except Exception as e:
            print("Error:", e)


if __name__ == "__main__":
    main()
