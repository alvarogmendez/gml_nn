# Compila cualquier ejemplo contra la librería de src/ (una sola copia):
#   make xor        make spiral        make diabetes_classifier
#   make            (todos)            make clean
CC       = gcc
CFLAGS   = -Wall -Wextra -O2 -I../src
LDLIBS   = -lm
LIB      = ../src/matrix.c ../src/gml_nn.c ../src/data_handler.c
EXAMPLES = $(basename $(wildcard *.c))

all: $(EXAMPLES)

%: %.c $(LIB)
	$(CC) $(CFLAGS) -o $@ $< $(LIB) $(LDLIBS)

clean:
	rm -f $(EXAMPLES)

.PHONY: all clean
