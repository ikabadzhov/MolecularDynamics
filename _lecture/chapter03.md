---
layout: default
title: "Chapter 03"
parent: Lecture
date: 2021-04-24
categories: lecture
author: Lars Pastewka
nav_order: 03
---
---

<h2 class='chapterHead'><span class='titlemark'>Chapter 3</span><br />
<a id='x1-10003'></a>Pair potentials</h2>
<div id='shaded*-1' class='framedenv'><!--  l. 4  -->
<p class='noindent'><span class='underline'><span class='cmbx-12'>Context:</span></span> Interatomic forces or interatomic potentials determine the material that we want to study. There is a plethora of interatomic potentials of varying accuracy, transferability and computational cost available in the literature. We here discuss simple pair potentials and point out algorithmic considerations.</p>
</div>
<h3 class='sectionHead'><span class='titlemark'>3.1</span> <a id='x1-20003.1'></a>Introduction</h3>
<!--  l. 10  -->
<p class='noindent'>The expression for \(E_\text{pot}\left (\{ \vec{r}_{i} \} \right )\) is the <span class='cmti-12'>model for the material</span> that we use in our molecular dynamics calculations. It determines whether we model water, proteins, metals, or any other physical object. Models are typically characterized by their <span class='cmti-12'>accuracy</span>, their <span class='cmti-12'>transferability</span> and the <span class='cmti-12'>computational cost</span> involved. (Computational cost also
includes the <span class='cmti-12'>computational complexity</span>.) At constant computational cost, there is always a tradeoff between accuracy and transferability. Accuracy and transferability can typically only be improved at the expense of additional computational cost.</p>
<ul class='itemize1'>
<li class='itemize'><span class='cmti-12'>Accuracy:</span> How close to we get to the true, measured value. For example, the absolute error of vacancy formation energy \(E_{\text{vac}} - E_{\text{vac}}^{\exp }\) with respect to experiment can be \(1\ \text{eV}\), \(0.1\ \text{eV}\) (typical), \(0.01\ \text{eV}\) (computationally expensive!). The vacancy formation energy is the energy required to remove a single atom from a solid. The resulting “hole” in the solid is called a vacancy.</li>
<li class='itemize'><span class='cmti-12'>Transferability:</span> Let’s assume we get the vacancy formation energy right to within \(0.1\ \text{eV}\) of the experimental value. Does the interstitial formation energy, i.e. the energy to insert an additional atoms between lattice sites, give the same value? If so, then the potential is transferable between these two situations. <span class='cmti-12'>Most interatomic potentials are not</span> <span class='cmti-12'>generally transferable,</span> and they need
to be tested when used in new situations, e.g. when the potential has been used to study crystals but you want to use it now to study a glass.</li>
<li class='itemize'><span class='cmti-12'>Computational cost:</span> The number of floating point operations determine how expensive it is to compute an energy or a force. (Nowadays, actual energy requirement for doing the calculation would be a better measure.) This is related to computational complexity, that says how the computational cost (i.e. the number of operations requires to compute the result) scales with the number of atoms. We want \(O(N)\) complexity, but many methods scale worse. Quantum
methods (tight-binding, density-functional theory) are usually \(O(N^{3})\) or worse.</li>
</ul>
<!--  l. 41  -->
<p class='noindent'></p>
<h3 class='sectionHead'><span class='titlemark'>3.2</span> <a id='x1-30003.2'></a>Pair potentials</h3>
<!--  l. 43  -->
<p class='noindent'>We have already encountered the simplest (and oldest) form of an interaction potentials, the pair potential. The total energy for a system interacting in <span class='cmti-12'>pairs</span> can be written quite generally as \begin{equation} E_\text{pot}\left ( \{ \vec{r}_{i} \} \right ) = \frac{1}{2}\sum _{i = 1}^{N}{\sum _{j = 1}^{N}{V\left ( r_{ij} \right ) = \sum _{i &lt; j}^{}{V(r_{ij})}}} \label{eq:pairpot} \end{equation} where \(r_{ij} = |\vec{r}_{i} - \vec{r}_{j}|\) is the
distance between atom <span class='cmti-12'>i</span> and atom <span class='cmti-12'>j</span>. \(V(r_{ij})\) is the pair interaction energy or just the pair potential and we assume that the interaction is pair-wise additive. The sum on the right (\(\sum _{i&lt;j}\)) runs over all pairs while sum on the left double counts each pair and therefore needs the factor \(1/2\). We have already seen a combination of the electrostatic potential and Pauli repulsion as an example of a pair-potential earlier.</p>
<!--  l. 55  -->
<p class='noindent'></p>
<h4 class='subsectionHead'><span class='titlemark'>3.2.1</span> <a id='x1-40003.2.1'></a>Dispersion forces</h4>
<!--  l. 57  -->
<p class='noindent'>An important type of interatomic and intermolecular is the London dispersion force. This interaction is attractive, and acts between all atoms even noble gases. Its lies in fluctuations of the atomic dipole moment. (This is a quantum mechanical effect, but the simplest model would be an electron orbiting a nucleus with a rotating dipole moment.) This fluctuating dipole <span class='cmti-12'>indices</span> a dipole in a second atom and these interact. The interaction decays as \(r^{-6}\)
at short distances. London dispersion forces are one of the forces that are often subsumed under the term van-der-Waals interaction.</p>
<!--  l. 59  -->
<p class='noindent'></p>
<h4 class='subsectionHead'><span class='titlemark'>3.2.2</span> <a id='x1-50003.2.2'></a>Lennard-Jones potential</h4>
<!--  l. 61  -->
<p class='noindent'>The Lennard-Jones potential combines dispersion forces with an empirical \(r^{- 12}\) model for Pauli repulsion. It is typically used for the interaction of noble atoms or molecules, i.e. systems that have closed electronic shells and therefore do not form covalent bonds. The interaction described by the Lennard-Jones potential are often called nonbonded interactions, because the typical interaction energy is on the order of \(k_B T\) (with room temperature for \(T\)). Thermal
fluctuation can thereby break this bond, hence the term nonbonded.</p>
<!--  l. 63  -->
<p class='indent'>One typical form to writing the Lennard-Jones potential is \begin{equation} V(r) = 4\varepsilon \left [ \left (\frac{\sigma }{r}\right )^{12} - \left (\frac{\sigma }{r}\right )^6 \right ] \end{equation} where \(\varepsilon \) is an energy and \(\sigma \) a length. The potential has a minimum as \(r=2^{1/6} \sigma \) and is repulsive for shorter distances and attractive for larger distances. For a noble gas (e.g. Argon), \(\varepsilon \sim 0.01\) eV and \(\sigma \sim 3\) Å.</p>
<!--  l. 69  -->
<p class='noindent'></p>
<h3 class='sectionHead'><span class='titlemark'>3.3</span> <a id='x1-60003.3'></a>Short-ranged potentials</h3>
<!--  l. 71  -->
<p class='noindent'>Implementing Eq. \eqref{eq:pairpot} naively leads to a complexity of \(O\left ( N^{2} \right )\) because the sum contains \(N^2\) terms. The trick is to cut the interaction range, i.e. set energies and forces to zero for distances larger than a certain cut-off distance \(r_c\). This is possible because the asymptotic behavior \(V\left ( r \right ) \rightarrow 0\) as \(r \rightarrow \infty \). Potentials for which this asymptotic decay is fast enough can be cut-off and are called
short-ranged. Note that we have already encountered a case in Chap. <span class='cmbx-12'>??</span> for which this is not possible, the Coulomb interaction that has the form \(V\left ( r \right ) \propto 1/r\).</p>
<!--  l. 78  -->
<p class='indent'>A simple way to see why this is not possible for the Coulomb interaction is to lump the charge-neutral infinite solid into charge-neutral dipoles. The effective interaction between dipoles then falls of as \(V^{\text{eff}}\left ( r \right ) \propto 1/r^{3}\). The contribution to the energy from all dipoles at distance \(r\) is \(V\left ( r \right )r^{2} \propto 1/r\). The full energy is obtained by integrating this function over \(r\), but the integral does not converge! This illustrates
the problem. The discrete sum is convergent, but only conditionally so, i.e. the outcome depends on the order of summation. We therefore can only cut interactions that decay as \(r^{-4}\) or faster.</p>
<!--  l. 85  -->
<p class='indent'>The potential energy with a cutoff looks as follows: \begin{equation} E_\text{pot}\left ( \{ \vec{r}_{i} \} \right ) = \frac{1}{2}\sum _{i = 1}^{N}{\sum _{\{ j|r_{ij} &lt; r_{c}\}}^{}{V\left ( r_{ij} \right )}} \label{eq:pairpotcut} \end{equation} The difference to Eq. \eqref{eq:pairpot} is that the second sum runs only over <span class='cmti-12'>neighbors</span> of \(i\), i.e. those atoms \(j\) whose distance \(r_{ij}&lt;r_c\) where \(r_c\) is the cutoff radius. This sum has
\(N\bar{n}\) elements where \(\bar{n}\) is a constant, is the average number of neighbors within the cutoff radius \(r_{c}\). The complexity of an algorithm that implements the above sum is hence \(O(N)\).</p>
<!--  l. 94  -->
<p class='indent'>A simple pair potential is often shifted at the cutoff to make the energy continuous (since \(V\left ( r_{c} \right ) \neq 0\)). The potential energy expression is then \(E_\text{pot} = \sum _{i &lt; j}^{}\left ( V\left ( r_{ij} \right ) - V\left ( r_{c} \right ) \right )\). Note that only by shifting the potential, forces and potential energy become consistent. Since only the forces affect the dynamics, the potential energy must be continuous and the integral of the forces, otherwise the
Hamiltonian \(H\) is not a conserved quantity. The shifted potential fulfills these requirements, the unshifted one does not.</p>
<!--  l. 99  -->
<p class='noindent'></p>
<h3 class='sectionHead'><span class='titlemark'>3.4</span> <a id='x1-70003.4'></a>Neighbor list search</h3>
<!--  l. 101  -->
<p class='noindent'>The sum Eq. \eqref{eq:pairpotcut} runs over all neighbors. One important algorithmic step with complexity \(O(N)\) in molecular dynamics codes is to build a <span class='cmti-12'>neighbor list</span>, i.e. find all pairs <span class='cmti-12'>i-j</span> with \(r_{ij} &lt; r_{c}\). This is usually done using a <span class='cmti-12'>domain</span> <span class='cmti-12'>decomposition</span> (see Fig. <a href='#x1-7001r1'>3.1<!--  tex4ht:ref: fig:neighborsearch   --></a>) that
divides the simulation domain in cells of a certain size and sorts all atoms into one of these cells. The neighbor list can then be constructed by looking for neighbors in neighboring cells only. If the cell size \(b\) is larger than the cutoff radius, \(b&gt;r_c\), then we only need to look exactly the neighboring cells.</p>
<figure class='figure'><!--  l. 111  -->
<p class='noindent'><img src='figures/neighbor_list_search.png' alt='PIC' height='390' width='390' /> <a id='x1-7001r1'></a> <a id='x1-7002'></a></p>
<figcaption class='caption'><span class='id'>Figure 3.1::</span> <span class='content'>Illustration of the typical data structure used for an \(O(N)\) neighbor search in a molecular dynamics simulation. For searching the neighbors within a cutoff \(r_c\) of the red atom, we only need to consider the candidate atoms that are in the cell adjacent to the red atom.</span></figcaption>
<!--  tex4ht:label?: x1-7001r3.4   --></figure>
<!--  l. 116  -->
<p class='indent'>We will here illustrate a typical neighbor search using the two-dimensional example shown in Fig. <a href='#x1-7001r1'>3.1<!--  tex4ht:ref: fig:neighborsearch   --></a>. Let us assume that each atom has a unique index \(i\in [1,N]\), where \(N\) is total number of atoms. (Attention, in C++ and other languages indices typically start at \(0\) and run to \(N-1\).) A typically algorithm first builds individual lists \(\{B_{k,mn}\}\) that contain the indices off all atoms in cell
\((m,n)\), i.e. \(k\in N_{nm}\) where \(N_{nm}\) is the number of atoms in this cell. The cell can simply be determined by dividing the position of the atom by the cell size \(b\), i.e. atom \(i\) resides in cell \(m_i=\lfloor x_i/b \rfloor \) and \(n_i=\lfloor y_i/b \rfloor \) where \(\lfloor \cdot \rfloor \) indicates the closest smaller integer. The lists \(\{B_{k,mn}\}\) are most conveniently stored in a single contiguous array; for purposes of accessing individual cells a second array is required that
stores the index of the first entry of cell \((m,n)\). Note that this second array is equal to the number of cells and can become prohibitively large when the system contains a lot of vacuum.</p>
<!--  l. 118  -->
<p class='indent'>The neighbor search then proceeds as follows: For atom \(i\), compute the cell \((m_i,n_i)\) in which this atom resides and then loop over all atoms in this cell and in cells \((m_i\pm 1,n_i)\), \((m_i, n_i\pm 1)\) and \((m_i\pm 1, n_i\pm 1)\). In two dimensions, this yields a loop over \(9\) cells, in three-dimensions there the loop runs over \(27\). If the distance between these two atoms is smaller than the cutoff \(r_c\), we add it to the neighbor list. Note that if the cell size
\(b\) is smaller than \(r_c\) we need include more cells in the search.</p>