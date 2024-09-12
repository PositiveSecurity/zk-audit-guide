# Audit of ZK protocols


## **For arithmetic circuits it’s necessary to check:**

- absence of redundant restrictions (which may lead to impossibility of valid proof calculation) in the circuit;
- existence of all necessary order constraints (<, >, =); for example, the remainder should be less than the divisor, the sum of the transaction inputs should be greater than the sum of the transaction outputs;
- prover’s inability to manipulate sensitive circuit information; in particular -- the variables, defining the scheme, should not be a part of the witnes; 
- involvement of all PI inputs and witness elements in the circuit;
- сircuit integrity (absence of logic-changing simplifications) after compilation.

## **For arithmetic circuits and smart contract functions/procedures:**

- checking input variables for being field element/having specified bit length;
- checking the arguments of mathematical functions for belonging to it’s domain (for example, modulo inverse argument should not be equal to 0, even if Fermat’s little theorem is used for calculation; O point should not be considered as lying on an elliptic curve), and the returned values for belonging to it’s range;
- checking that during arithmetic operations an intermediate result belongs to field / not exceeds the bit length of the variable.

## **For contracts:**

    - checking the quality of pseudo-random variables (in particular, blinding polynomials of the Plonk protocol);
    - checking the reliability of the cryptoprimitives used and sets of their settings (key length, number of rounds…);
    - checking the correctness of resources locking during shared access: no one (even the same user) should have opportunity to initiate the next operation on the data before the previous one is finished;
    - checking that after updating the value of a certain variable, all memory areas associated with it are updated.

## **Protocol-specific:**

    - checking the determinism of the nullifier calculation (given the same initial data, the same result is always obtained);
    - checking that the degree of a polynomial is determined correctly (monomials of higher degrees with zero coefficients are not taken into account);
    - checking that strong Fiat-Shamir transformation is used to obtain random protocol variables: Public Input and common parameters are fed to the hash function during it’s initialization;
    - checking that the proof for the groth16 protocol is protected from modification [4];
    - checking that no sensitive data is leaked during the trusted installation procedure.
