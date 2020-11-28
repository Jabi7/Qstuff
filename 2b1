def week2b_ans_func(lightout4):
    ##### Build your cirucuit here
    ####  In addition, please make sure your function can solve the problem with different inputs (lightout4). We will cross validate with different inputs.
    
    def diffuser(nqubits):
            qc = QuantumCircuit(nqubits)
            # Apply transformation |s> -> |00..0> (H-gates)
            for qubit in range(nqubits):
                qc.h(qubit)
            # Apply transformation |00..0> -> |11..1> (X-gates)
            for qubit in range(nqubits):
                qc.x(qubit)
            # Do multi-controlled-Z gate
            qc.h(nqubits-1)
            qc.mct(list(range(nqubits-1)), nqubits-1)  # multi-controlled-toffoli
            qc.h(nqubits-1)
            # Apply transformation |11..1> -> |00..0>
            for qubit in range(nqubits):
                qc.x(qubit)
            # Apply transformation |00..0> -> |s>
            for qubit in range(nqubits):
                qc.h(qubit)
            # We will return the diffuser as a gate
            U_s = qc.to_gate()
            U_s.name = "$U_s$"
            return U_s    
    
    def counter(qc, f, a):
        for i in range(len(f)):
            qc.mct([f[i],a[0],a[1]],a[2],a[3], mode = 'noancilla')
            qc.mct([f[i],a[0],a[1]],a[2], mode = 'noancilla')
            qc.ccx(f[i], a[0],a[1])
            qc.cx(f[i], a[0])
    def rcounter(qc, f, a):
        for i in range(len(f)):
            qc.cx(f[i], a[0])
            qc.ccx(f[i], a[0],a[1])
            qc.mct([f[i],a[0],a[1]],a[2], mode = 'noancilla')
            qc.mct([f[i],a[0],a[1]],a[2],a[3], mode = 'noancilla')
            
    def qram(qc, address, v, lightsout4):
    # address 0 -> data = 1
        qc.x([address[0],address[1]])
        for i in range(9):
            if lightsout4[0][i]==1:
                qc.ccx(address[0],address[1],v[i])
        qc.x([address[0],address[1]])
        qc.barrier()
        # address 1 -> data = 2
        qc.x(address[0])
        for i in range(9):
            if lightsout4[1][i]==1:
                qc.ccx(address[0],address[1],v[i])
        qc.x(address[0])
        qc.barrier()
        # address 2 -> data = 5
        qc.x(address[1])
        for i in range(9):
            if lightsout4[2][i]==1:
                qc.ccx(address[0],address[1],v[i])
        qc.x(address[1])
        qc.barrier()
        # address 3 -> data = 7
        for i in range(9):
            if lightsout4[3][i]==1:
                qc.ccx(address[0],address[1],v[i])
    
    
    address = QuantumRegister(2, name = 'addr')
    v = QuantumRegister(9, name = 'data')
    c = QuantumRegister(9, name = 'flip')
    cl = ClassicalRegister(2, name = 'cl')
#     o= QuantumRegister(1, name='out')
    a= QuantumRegister(4, name='aux')
    
    fl= QuantumRegister(1, name='flag')
    qc = QuantumCircuit(address,c,v,a,fl,cl)

    # address preparation
    qc.h([address[0],address[1]])
    qc.h(v)
#     qc.x(o[0])
#     qc.h(o[0])
    qc.x(fl[0])
    qc.h(fl[0])
    qc.barrier()
    
    for i in range(3):
        qram(qc, address, c, lightsout4)
        qc.barrier()
        # address 3 -> data = 7
        
        qc.barrier()
        
        # U_2a
        for i in range(12):    
            
            qc.cx(v[0], [c[0],c[1],c[3]])
        
            qc.cx(v[1], [c[1],c[0],c[2],c[4]])

            qc.cx(v[2], [c[2],c[1],c[5]])

            qc.cx(v[3], [c[3],c[0],c[4], c[6]])

            qc.cx(v[4], [c[4],c[1],c[3],c[5],c[7]])

            qc.cx(v[5], [c[5],c[2],c[4],c[8]])

            qc.cx(v[6], [c[6],c[3],c[7]])

            qc.cx(v[7], [c[7],c[6],c[4],c[8]])

            qc.cx(v[8], [c[8],c[7],c[5]])
            qc.x(c)

            qc.mct(c, fl)
            
            qc.x(c)
            qc.cx(v[0], [c[0],c[1],c[3]])
        
            qc.cx(v[1], [c[1],c[0],c[2],c[4]])

            qc.cx(v[2], [c[2],c[1],c[5]])

            qc.cx(v[3], [c[3],c[0],c[4], c[6]])

            qc.cx(v[4], [c[4],c[1],c[3],c[5],c[7]])

            qc.cx(v[5], [c[5],c[2],c[4],c[8]])

            qc.cx(v[6], [c[6],c[3],c[7]])

            qc.cx(v[7], [c[7],c[6],c[4],c[8]])

            qc.cx(v[8], [c[8],c[7],c[5]])

            qc.append(diffuser(9), v)

        qc.barrier()
        
        # counter
        counter(qc, c, a)
        qc.x([a[0],a[1]])
        qc.ccx(a[2],a[3], fl[0])
        qc.x([a[0],a[1]])
        
        counter(qc, c, a)
        qc.barrier()
        
        # U-2a dagger
        for i in range(12):    

            qc.append(diffuser(9), v)
            
            
            qc.cx(v[0], [c[0],c[1],c[3]])
        
            qc.cx(v[1], [c[1],c[0],c[2],c[4]])

            qc.cx(v[2], [c[2],c[1],c[5]])

            qc.cx(v[3], [c[3],c[0],c[4], c[6]])

            qc.cx(v[4], [c[4],c[1],c[3],c[5],c[7]])

            qc.cx(v[5], [c[5],c[2],c[4],c[8]])

            qc.cx(v[6], [c[6],c[3],c[7]])

            qc.cx(v[7], [c[7],c[6],c[4],c[8]])

            qc.cx(v[8], [c[8],c[7],c[5]])
            qc.x(c)

            qc.mct(c, fl)
            
            qc.x(c)
            qc.cx(v[0], [c[0],c[1],c[3]])
        
            qc.cx(v[1], [c[1],c[0],c[2],c[4]])

            qc.cx(v[2], [c[2],c[1],c[5]])

            qc.cx(v[3], [c[3],c[0],c[4], c[6]])

            qc.cx(v[4], [c[4],c[1],c[3],c[5],c[7]])

            qc.cx(v[5], [c[5],c[2],c[4],c[8]])

            qc.cx(v[6], [c[6],c[3],c[7]])

            qc.cx(v[7], [c[7],c[6],c[4],c[8]])

            qc.cx(v[8], [c[8],c[7],c[5]])
        qc.barrier()

        qram(qc, address, c, lightsout4)
        qc.barrier()
    
    #diffusion
    qc.h(address[:2])
    qc.x(address[:2])
    qc.h(address[1])
    qc.cx(address[0], address[1])
    qc.h(address[1])
    qc.x(address[:2])
    qc.h(address[:2])
    
    qc.h(fl[0])
    qc.x(fl[0])
    qc.measure(address[1], cl[0])
    qc.measure(address[0], cl[1])
    qc = qc.reverse_bits()
    return qc



# k = week2b_ans_func(lightsout4)
# k.draw()
