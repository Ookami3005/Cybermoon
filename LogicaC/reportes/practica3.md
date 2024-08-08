[<- Índice](indiceReportesLogica.md)
# Formas normales

## Integrantes

- Legorreta Esparragoza Juan Luis
- Rocha Salazar Mario Alberto
- Romero Cruz Fernando

## Funciones

### Funciones auxiliares

- La función `fncAux` busca devolver una fórmula equivalente a la recibida donde las conjunciones se apliquen solo a otras conjunciones, variables, variables negadas, o en su defecto a disyunciones de otras disyunciones o variables negadas o sin negar. Por tanto se aplica distributividad de ser necesario para obtener la fórmula buscada

> Recalcamos que esta función fue pensada para que unicamente recibiera fórmulas en forma normal negativa, de recibir una fóormula que no cumpla este prerequisito, se obtendria un error por la falta de patrones exhaustivos.

```haskell
-- Función que convierte una fórmula normal negativa a forma normal conjuntiva.
fncAux :: Prop -> Prop
fncAux (Var p) = Var p
fncAux (Neg (Var p)) = Neg (Var p)
fncAux (Conj p q) = Conj (fncAux p) (fncAux q)

-- Casos en los que recibe una fórmula en disyunción.
-- Se aplica la distribución de conjunción sobre disyunción de ser necesario
fncAux (Disy p q) = case (p,q) of (Conj p1 p2, Conj q1 q2) -> Conj (Conj (Conj (fncAux (Disy p1 q1)) (fncAux (Disy p1 q2))) (fncAux (Disy p2 q1))) (fncAux (Disy p2 q2))
                                  (Conj p1 p2, _) -> Conj (fncAux (Disy p1 q)) (fncAux (Disy p2 q))
                                  (_, Conj q1 q2) -> Conj (fncAux (Disy p q1)) (fncAux (Disy p q2))
                                  -- Nos aseguramos que las disyunciones solo contengan disyunciones o variables
                                  (Disy p1 p2, _) -> case (fncAux p, fncAux q) of (Conj p1 p2, _) -> fncAux (Disy (fncAux p) (fncAux q))
                                                                                  (_, Conj q1 q2) -> fncAux (Disy (fncAux p) (fncAux q))
                                                                                  (_, _) -> Disy p q
                                  (_, Disy q1 q2) -> case (p, fncAux q) of (_, Conj q1 q2) -> fncAux (Disy p (fncAux q))
                                                                           (_, _) -> Disy p q
                                  (_, _) -> Disy p q
```

- La función `fndAux` busca devolver una fórmula equivalente a la recibida donde las disyunciones se apliquen solo a otras disyunciones, variables, variables negadas, o en su defecto a conjunciones de otras conjunciones o variables negadas o sin negar. Por tanto se aplica distributividad de ser necesario para obtener la fórmula buscada

> Recalcamos que esta función fue pensada para que unicamente recibiera fórmulas en forma normal negativa, de recibir una fóormula que no cumpla este prerequisito, se obtendria un error por la falta de patrones exhaustivos.

```haskell
-- Función que convierte una fórmula normal negativa a forma normal disyuntiva.
fndAux :: Prop -> Prop
fndAux (Var p) = Var p
fndAux (Neg (Var p)) = Neg (Var p)

-- Casos en los que recibe una fórmula en conjunción.
-- Se aplica la distribución de disyunción sobre conjunción de ser necesario
fndAux (Conj p q) = case (p,q) of (Disy p1 p2, Disy q1 q2) -> Disy (Disy (Disy (fndAux (Conj p1 q1)) (fndAux (Conj p1 q2))) (fndAux (Conj p2 q1))) (fndAux (Conj p2 q2))
                                  (Disy p1 p2, _) -> Disy (fndAux (Conj p1 q)) (fndAux (Conj p2 q))
                                  (_, Disy q1 q2) -> Disy (fndAux (Conj p q1)) (fndAux (Conj p q2))
                                  -- Nos aseguramos que las conjunciones solo contengan conjunciones o variables
                                  (Conj p1 p2, _) -> case (fndAux p, fndAux q) of (Disy p1 p2, _) -> fndAux (Conj (fndAux p) (fndAux q))
                                                                                  (_, Disy q1 q2) -> fndAux (Conj (fndAux p) (fndAux q))
                                                                                  (_, _) -> Conj p q
                                  (_, Conj q1 q2) -> case (p, fndAux q) of (_, Disy q1 q2) -> fndAux (Conj p (fndAux q))
                                                                           (_, _) -> Conj p q
                                  (_, _) -> Conj p q
```

### Funciones principales

- La función `fnn` devuelve una fórmula equivalente a la recibida, pero en forma normal negativa, por lo que las negaciones de proposiciónes en la fórmula recibida se desarrollan mediante ==DeMorgan== de tal manera que las unicas operaciónes lógicas en la fórmula final sean ==conjunciones==  y ==disyunciones==, y que las negaciones, de haberlas, solo se apliquen sobre variables.

```haskell
-- Función que dada una fórmula f, regresa una fórmula equivalente a f
-- en su Forma Normal Negativa.
fnn :: Prop -> Prop

-- Especificación de los casos en los que recibe una fórmula en negación.
fnn (Neg prop) = case prop of (Var p) -> Neg prop
                              (Neg p) -> fnn p
                              (Conj p q) -> Disy (fnn (Neg p)) (fnn (Neg q))
                              (Disy p q) -> Conj (fnn (Neg p)) (fnn (Neg q))
                              (Impl p q) -> Conj (fnn p) (fnn (Neg q))
                              (Syss p q) -> Disy (Conj (fnn p) (fnn (Neg q))) (Conj (fnn q) (fnn (Neg p)))

-- Especificación de los casos en los que recibe una fórmula sin negación.
fnn prop = case prop of (Var p) -> prop
                        (Conj p q) -> Conj (fnn p) (fnn q)
                        (Disy p q) -> Disy (fnn p) (fnn q)
                        (Impl p q) -> Disy (fnn (Neg p)) (fnn q)
                        (Syss p q) -> Conj (Disy (fnn (Neg p)) (fnn q)) (Disy (fnn p) (fnn (Neg q)))
```

- La función `fnc` devuelve una fórmula equivalente a la recibida pero en forma normal conjuntiva
> Su funcionamiento recae totalmente sobre la función auxiliar `fncAux`, aqui simplemente nos encargamos, por temas de complejidad, de reducir la fórmula a forma normal negativa para que dicha función no tenga problemas en su funcionamiento ni tenga que estar calculando varias veces la forma normal negativa.

```haskell
-- Función que dada una fórmula cualquiera f, regresa una fórmula equivalente a f
-- en su Forma Normal Conjuntiva.
fnc :: Prop -> Prop
-- Obtenemos una fórmula equivalente en forma normal negativa y después la convertimos a
-- forma normal conjuntiva con el método auxiliar.
fnc prop = fncAux (fnn prop)
```

- La función `fnd` devuelve una fórmula equivalente a la recibida pero en forma normal disyuntiva
> Su funcionamiento recae totalmente sobre la función auxiliar `fndAux`, aqui simplemente nos encargamos, por temas de complejidad, de reducir la fórmula a forma normal negativa para que dicha función no tenga problemas en su funcionamiento ni tenga que estar calculando varias veces la forma normal negativa.

```haskell
-- Función que dada una fórmula cualquiera f, regresa una fórmula equivalente a f
-- en su Forma Normal Conjuntiva.
fnc :: Prop -> Prop
-- Obtenemos una fórmula equivalente en forma normal negativa y después la convertimos a
-- forma normal conjuntiva con el método auxiliar.
fnc prop = fncAux (fnn prop)
```