# Compila la librería como estática: libgml_nn.a
#   make            make clean
# Uso desde un programa:  gcc -I<ruta>/src mi_programa.c -L<ruta>/src -lgml_nn -lm
CC     = gcc
CFLAGS = -Wall -Wextra -O2
OBJ    = matrix.o gml_nn.o data_handler.o

libgml_nn.a: $(OBJ)
	ar rcs $@ $^

%.o: %.c
	$(CC) $(CFLAGS) -c $< -o $@

clean:
	rm -f $(OBJ) libgml_nn.a

.PHONY: clean
