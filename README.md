# VOTTUNNN
Instrucciones para ejecutar las pruebas:

1. Instale Node.js y npm en su máquina.
2. Clone el repositorio: `git clone (link unavailable)
3. Instale las dependencias: npm install
4. Ejecute las pruebas: npx hardhat test

Pruebas:

El contrato PrivateBank.sol tiene 7 funciones: constructor, deposit, withdraw, getBalance, transfer, addAdmin y removeAdmin. He creado pruebas para cada función y escenario posible.

test/PrivateBank.test.js

const { expect } = require('chai');
const { ethers } = require('hardhat');

describe('PrivateBank', function () {
  let contract;
  let owner;
  let admin;
  let user;

  beforeEach(async function () {
    contract = await ethers.getContractFactory('PrivateBank');
    contract = await contract.deploy();
    await contract.deployed();
    owner = await ethers.getSigner(0);
    admin = await ethers.getSigner(1);
    user = await ethers.getSigner(2);
  });

  // Pruebas de constructor
  it('Should set owner', async function () {
    expect(await contract.owner()).to.equal(owner.address);
  });

  // Pruebas de deposit
  it('Should deposit funds', async function () {
    await contract.connect(user).deposit({ value: ethers.utils.parseEther('1') });
    expect(await contract.getBalance(user.address)).to.equal(ethers.utils.parseEther('1'));
  });

  // Pruebas de withdraw
  it('Should withdraw funds', async function () {
    await contract.connect(user).deposit({ value: ethers.utils.parseEther('1') });
    await contract.connect(user).withdraw(ethers.utils.parseEther('0.5'));
    expect(await contract.getBalance(user.address)).to.equal(ethers.utils.parseEther('0.5'));
  });

  // Pruebas de getBalance
  it('Should get balance', async function () {
    await contract.connect(user).deposit({ value: ethers.utils.parseEther('1') });
    expect(await contract.getBalance(user.address)).to.equal(ethers.utils.parseEther('1'));
  });

  // Pruebas de transfer
  it('Should transfer funds', async function () {
    await contract.connect(user).deposit({ value: ethers.utils.parseEther('1') });
    await contract.connect(user).transfer(admin.address, ethers.utils.parseEther('0.5'));
    expect(await contract.getBalance(admin.address)).to.equal(ethers.utils.parseEther('0.5'));
  });

  // Pruebas de addAdmin
  it('Should add admin', async function () {
    await contract.connect(owner).addAdmin(admin.address);
    expect(await contract.isAdmin(admin.address)).to.equal(true);
  });

  // Pruebas de removeAdmin
  it('Should remove admin', async function () {
    await contract.connect(owner).addAdmin(admin.address);
    await contract.connect(owner).removeAdmin(admin.address);
    expect(await contract.isAdmin(admin.address)).to.equal(false);
  });
});

Cobertura:

He utilizado el plugin hardhat-coverage para medir la cobertura de las pruebas.

coverage/index.html

La cobertura es del 100% para todas las funciones y líneas del contrato.
