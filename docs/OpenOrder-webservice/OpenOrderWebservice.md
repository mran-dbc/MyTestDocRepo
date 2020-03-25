# Description of OpenOrder-webservice

## Introduction
This repo contains the OpenOrder webservice and client. It is part of the Open Library System Suite.

The Open Order web service is a service for requesting materials that are either owned by the user's library or is not owned by the library (inter library loan). The policies for ordering materials are based on those used by bibliotek.dk, and are controlled by the agencies (libraries) in the VIP database.

The OpenOrder service serves several functions related to ordering of materials managed by libraries.
The service can provide definitive information on whether a specific resource can be ordered, it can receive orders and pass them on for further processing in the OLS Base Over Bestillinger (BOB) system, and it can update the status of registered orders.

Supported protocols are SOAP over HTTPS.

The service consists of two operations: checkOrderPolicy and PlaceOrder.

CheckOrderPolicy checks that a given agency will allow for enduser ill on the material. If the pid is omitted, the operation will check that a given agency allows for ill.

PlaceOrder creates an order in the orderSystem. It implicitly calls checkOrderPolicy to test if the given agency accepts ill on the pid supplied.

Web Service: https://openorder.addi.dk/
Example client: https://openorder.addi.dk/copa-rs/

## License Terms
Use of the web service requires [subscription of a DanBib license](http://www.dbc.dk/produkter-services/databaser_tjenester_produktoversigt/danbib) (Netpunkt password).
The web service is published under the GPLv3 License.

## License
DBC-Software Copyright ?. 2009, Danish Library Center, dbc as.

This library is Open source middleware/backend software developed and distributed under the following licenstype:

GNU, General Public License Version 3. If any software components linked together in this library have legal conflicts with distribution under GNU 3 it will apply to the original license type.

Software distributed under the License is distributed on an "AS IS" basis, WITHOUT WARRANTY OF ANY KIND, either express or implied. See the License for the specific language governing rights and limitations under the License.

Around this software library an Open Source Community is established. Please leave back code based upon our software back to this community in accordance to the concept behind GNU.

You should have received a copy of the GNU Lesser General Public License along with this library; if not, write to the Free Software Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301  USA
